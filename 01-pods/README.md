
## PODs

Un Pod es un recurso de Kubernetes que encapsula contenedores. 

Puedes crear un Pod desplegando una imagen con un comando kubectl. Esto crea automáticamente un Deployment, que a su vez genera un Pod y un ReplicaSet.

```
kubectl run nginx --image nginx
```

o bien 

```
kubectl run --generator=run-pod/v1 nginx --image=nginx
```

Otra forma de crear Pods es mediante un archivo Yaml (manifiesto), el cual se aplica con el comando kubectl apply

```
kubectl apply -f 01-pod-definition.yaml 
```

Para ver los Pods del cluster debes ejecutar 

```
kubectl get pods
```
