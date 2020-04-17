

## Namespaces

Los recursos de Kubernetes se alojan por defecto en un espacio denominado Namespaces. Aquí se crean nuestros Pods, Services, ReplicaSet, etc.

Para crear un Namespace se utiliza el comando:

```
kubectl create namespace dev
```

Para trabajar en ese Namespace:

```
kubectl config set-context $(kubectl config current-context) --namespace=dev
```

Para ver tus Namespaces:

```
kubectl get ns
```

Para ver los objetos de un Namespace en particular:

```
kubectl get pods -n dev
kubectl get pods --namespace=dev
```