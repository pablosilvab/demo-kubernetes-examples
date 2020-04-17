

## Deployment

En un Deployment se define el estado deseado de Pods y ReplicaSets. Es posible generar un Deployment mediante el comando kubectl run o a través de un manifiesto.

```
kubectl create -f 01-deploy-definition.yaml 
```

En el Deployment puedes definir:
- Número de réplicas 
- Nombre de la imagen
- Nombre del contenedor
- Uso de recursos del contenedor
- Puertos a utilizar

Puedes actualizar tu deployment con el siguiente comando:

```
kubectl set image deployment/nginx-deployment nginx=nginx:1.16.1 --record
```