# ejercicio_07

Primero generar el archivo configmap-env.yaml

apiVersion: v1
kind: ConfigMap
metadata:
  name: pingapp-config-env
  labels:
    app: pingapp
data:
  CONFIG_LOCATION: /config/config.json
  
  
  Luego ejecutar: $kubectl -n arba apply -f configmap-env.yaml
  
  
 
Luego, el segundo archivo de deployment, donde se especifican las replicas ( en este caso 3 ).
  
  
$kubectl -n arba apply deployment-v2.yaml
  
apiVersion: v1
kind: ConfigMap
metadata:
  name: pingapp-config-env
  labels:
    app: pingapp
data:
  CONFIG_LOCATION: /config/config.jsonMacBook-Pro-de-Federico:2088421 fede$ cat deployment-v2.yaml 
apiVersion: apps/v1
kind: Deployment
metadata:
  name: pingapp
  labels:
    app: pingapp
spec:
  replicas: 3
  selector:
    matchLabels:
      app: pingapp
  template:
    metadata:
      labels:
        app: pingapp
    spec:
      containers:
      - name: pingapp
        image: nicopaez/pingapp:2.1.0
        ports:
        - containerPort: 4567
        envFrom:
          - configMapRef:
              name: pingapp-config-env
