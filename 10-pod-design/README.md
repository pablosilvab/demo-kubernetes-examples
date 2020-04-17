# Pod Design

## Labels
Los Labels permiten clasificar a los Pods por alguna característica, por ejemplo su función: backend o frontend.

Ejemplo:
```
metadata:
  name: nginx
  labels:
    app: App1
    function: backend
``` 

## Selectors
Los Selectors pson utilizados por los recursos de Kubernetes para asociar este recurso a una aplicación.

Ejemplo:
```
apiVersion: v1
kind: Service
metadata:
  name: nginx
spec:
  selector:
    app: App1
  ports:
  - port: 80
    targetPort: 8080

```

## Annotations
Las Annotations sirven para almacenar información de métricas para otros propósitos.

Ejemplo: 
```
metadata:
  name: nginx
  labels:
    app: App1
    function: backend
  annotations:
    buildVersion: "1.0"
```
