
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