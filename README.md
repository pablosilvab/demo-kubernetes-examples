# Demo Kubernetes
El objetivo de este proyecto es disponibilizar ejemplos básicos de los recursos de Kubernetes

## PODs

Puedes crear un Pod desplegando una imagen con un comando kubectl. Esto crea automáticamente un Deployment, que a su vez genera un Pod y un ReplicaSet.

```
kubectl run nginx --image nginx
```

Otra forma de crear Pods es mediante un archivo Yaml (manifiesto), el cual se aplica con el comando kubectl apply

```
kubectl apply -f pods/01-pod-definition.yaml 
```

