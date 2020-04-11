# Demo Kubernetes
El objetivo de este proyecto es disponibilizar ejemplos básicos de los recursos de Kubernetes

## Contenido

- [PODs](#pods)
- [ReplicaSet](#replicaset)
- [Deployment](#deployment)

## PODs

Un Pod es un recurso de Kubernetes que encapsula contenedores. 

Puedes crear un Pod desplegando una imagen con un comando kubectl. Esto crea automáticamente un Deployment, que a su vez genera un Pod y un ReplicaSet.

```
kubectl run nginx --image nginx
```

Otra forma de crear Pods es mediante un archivo Yaml (manifiesto), el cual se aplica con el comando kubectl apply

```
kubectl apply -f 01-pod-definition.yaml 
```

Para ver los Pods del cluster debes ejecutar 

```
kubectl get pods
```

## ReplicaSet

El objetivo de un objeto ReplicaSet es definir el número de réplicas de Pods ejecutándose al mismo tiempo. Lo puedes definir con el comando kubectl run o en un manifiesto. 

* Mediante kubectl run: 

```
kubectl run nginx --replicas=2 --image nginx 
```

* Mediante kubectl apply:

```
kubectl apply -f 01-replicaset-definition.yaml
```

* Puedes cambiar el número de réplicas con el siguiente comando:

```
kubectl scale --replicas=7 replicaset nginx-replicaset
```

* Puedes eliminar el ReplicaSet y eliminar los Pods subyacentes con el comando para eliminar recursos

```
kubectl delete rs nginx-replicaset
```

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


