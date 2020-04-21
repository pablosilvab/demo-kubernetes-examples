
## Services
Un Service permite que exista tráfico hacia componentes externos de la aplicación basados en un label.

### NodePort
Crea un puerto en el Nodo que accede al Pod.

Ejemplo: Wordpress
```
kubectl run wordpress --image=wordpress:php7.1-apache --port=80
```

Service NodePort 
```
apiVersion: v1
kind: Service
metadata:
  name: wordpress 
spec:
  type: NodePort
  ports:
  - port: 80 
    targetPort: 80
    nodePort: 30000
  selector:
    role: wordpress 
```

Al ejecutar ```kubectl get svc``` vemos:

```
NAME         TYPE        CLUSTER-IP     EXTERNAL-IP   PORT(S)        AGE
wordpress    NodePort    10.245.249.55   <none>        80:30000/TCP   67s
```

Para el ejemplo, se puede testear con:

```
curl http://<node-external-ip>:30000
```


### ClusterIP
IP Virtual dentro del cluster para que haya comunicación entre los diferentes Services solamente dentro del cluster. Podría servir cuando quiero acceder desde un Pod que está en el backend a un Pod que está en el frontend. Esto se puede hacer utilizando los nombres de los Services.

```
apiVersion: v1
kind: Service
metadata:
  name: wordpress-cluster
spec:
  ports:
  - port: 80 
    targetPort: 80
  selector:
    role: wordpress 
```

Al aplicar ```kubectl get svc```:

```
NAME                 TYPE           CLUSTER-IP      EXTERNAL-IP       PORT(S)        AGE
wordpress-cluster    ClusterIP      10.245.30.166   <none>            80/TCP         8m14s
```

### LoadBalancer
Provee un balanceador de carga para nuestra aplicación. (Proveedores Cloud) 

```
apiVersion: v1
kind: Service
metadata:
  name: wordpress-lb
spec:
  type: LoadBalancer
  ports:
    - protocol: TCP
      port: 80
      targetPort: 80
      name: http
  selector:
    role: wordpress
```

Al aplicar ```kubectl get svc```:

```
NAME                 TYPE           CLUSTER-IP      EXTERNAL-IP       PORT(S)        AGE
wordpress-lb         LoadBalancer   10.245.85.19    134.209.128.221   80:31918/TCP   2m41s
```
