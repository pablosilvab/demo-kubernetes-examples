# Demo Kubernetes
El objetivo de este proyecto es disponibilizar ejemplos básicos de los recursos de Kubernetes

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

## ReplicaSet

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


