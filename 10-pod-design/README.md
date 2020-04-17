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
Los Selectors son utilizados por los recursos de Kubernetes para asociar este recurso a una aplicación.

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

## Rolling Update & Rollbacks

Cuando en Kubernetes se crea un Deployment se dispara un rollout, el cual genera una revisión que se puede denominar "Revision 1".

Cuando se actualiza el Deployment, por ejemplo, al cambiar una imagen, se dispara un nuevo rollout, generando una "Revision 2".

Para ver el estado de estos rollout, existe un comando ```kubectl```:

```
kubectl rollout status deployment/myapp
```


Para ver el histórico de revisiones:
```
kubectl rollout history deployment/myapp
```

* Ejemplo: 

Aplicar Deployment:
```
kubectl apply -f 04-deployment-rollout.yaml 
```

Al aplicar ```kubectl rollout status ``` el output es:

```
Waiting for deployment "myapp" rollout to finish: 0 of 2 updated replicas are available...
Waiting for deployment "myapp" rollout to finish: 1 of 2 updated replicas are available...
deployment "myapp" successfully rolled out
```

Cambiar imagen:
```
kubectl set image deployment/myapp nginx=nginx:1.9.1
```

Al aplicar ```kubect rollout history``` tenemos dos revisiones en el output.

```
deployment.apps/myapp 
REVISION  CHANGE-CAUSE
1         <none>
2         <none>
```

Para que el campo CHANGE-CAUSE no se muestre vacío se debe utilizar el flag ```--record```.

```
kubectl set image deployment nginx nginx=nginx:1.17 --record
```

Ejemplo:
```
deployment.apps/myapp 
REVISION  CHANGE-CAUSE
2         <none>
3         <none>
4         kubectl set image deployment/myapp nginx-controller=nginx:1.17 --record=true
```

Si inspeccionamos el Deployment podemos ver que la estrategia Rolling Update lo que hizo fue eliminar un Pod del primer rollout, para luego crear un con la imagen que actualizamos. 

```
Events:
  Type    Reason             Age    From                   Message
  ----    ------             ----   ----                   -------
  Normal  ScalingReplicaSet  2m12s  deployment-controller  Scaled up replica set myapp-67b6b8548d to 2
  Normal  ScalingReplicaSet  116s   deployment-controller  Scaled up replica set myapp-55d675cd5c to 1
  Normal  ScalingReplicaSet  98s    deployment-controller  Scaled down replica set myapp-67b6b8548d to 1
  Normal  ScalingReplicaSet  98s    deployment-controller  Scaled up replica set myapp-55d675cd5c to 2
  Normal  ScalingReplicaSet  94s    deployment-controller  Scaled down replica set myapp-67b6b8548d to 0
```

## Rollback
Para realizar un rollback en Kubernetes, se debe aplicar el comando.
```
kubectl rollout undo deployment/myapp
```