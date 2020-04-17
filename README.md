# Demo Kubernetes
El objetivo de este proyecto es disponibilizar ejemplos básicos de los recursos de Kubernetes

## Requerimientos 📋

- Tener Kubernetes instalado
- Un cluster Kubernetes disponible

## Contenido

- [PODs](#pods)
- [ReplicaSet](#replicaset)
- [Deployment](#deployment)
- [Namespaces](#namespaces)
- [Configuración](#configuracion)
- [Variables de ambiente](#envvars)
- [ConfigMap](#configmap)
- [Secrets](#secrets)
- [Service Account](#serviceaccount)
- [Resources](#resources)
- [Taints & Tolerations](#taints)

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

## Configuración

### Comandos y argumentos

Puedes definir comandos y argumentos para los contenedores que se ejecuten en un Pod. Estos no se pueden cambiar después de que el Pod es creado.

```
  containers:
  - name: debian-command
    image: debian
    command: ["printenv"]
    args: ["HOSTNAME", "KUBERNETES_PORT"]
```

### EnvVars

Puedes definir variables de ambiente para los contenedores que se ejecutan en un Pod.

```
kubectl exec -it hello-world -- /bin/bash
```

En la shell puedes ejecutar el comando ```printenv``` para ver las variables de ambiente.

### ConfigMap

ConfigMaps permiten desacoplar información de contiguración de la imagen. Puedes crearlos de varias formas.

* Forma #1:

```
kubectl create configmap myconfigmap1 --from-literal=WORLD=DeveloperfromliteralConfigMap
```

* Forma #2:

```
kubectl create configmap myconfigmap2 --from-file=06-app.properties
```

* Forma #3: Aplicar un manifiesto 

### Secrets

* Codificar valores

```
echo -n 'mysql' | base64
```

* Decodificar valores

```
echo -n 'bXlzcWw=' | base64 --decode 
```

* Creación de un Secret: Forma imperativa

```
kubectl create secret generic app-secret-1 --from-literal=DB_Host=mysql --from-literal=DB_Password=passwd
```

Esto es inseguro ya que datos sensibles están expuestos.

### ServiceAccount

Creación de Service Account de forma imperativa:

```
kubectl create serviceaccount dashboard-sa
```

Esto genera un token asociado al Service Account, que es almacenado en un Secret. 

### Resources

* Resource Request: Mínimo de memoria y CPU requerido por el contenedor cuando el scheduler de Kubernetes intenta ubicar el Pod en un nodo.

* Resource Limits: Define límites de uso de memoria y CPU para un contenedor.

Estos recursos son definidos para cada contenedor dentro del Pod.

```
    resources:
      requests:
        memory: "64Mi"
        cpu: "250m"
      limits:
        memory: "128Mi"
        cpu: "500m"
```

### Taints

Los Taints & Tolerations no tienen que ver con la seguridad de los Nodos o los Pods, son simplemente restricciones asociadas a estos. Los Tains se definen en los Nodos y los Tolerations en los Pods.

¿Cómo funciona este comportamiento?
- Se define en un Nodo un Tains, con el objetivo de restringir el acceso a dicho nodo. taint=blue
- En el Pod se define un Tolerations, el cual debe ser tolerante al Taint.

Definición

```
kubectl taint nodes node-name key=value:taint-effect
```

```
kubectl taint nodes node1 app=blue:NoSchedule | PreferNoSchedule | NoExecute
```

Borrar Taints:

```
kubectl taint nodes minikube app=blue:NoSchedule-
```


| Taint Effect | Status Pod|
| ------------- | ------------- |
| NoSchedule  | Pending  |
| PreferNoSchedule  | Running  |
| NoExecute | Pending  |

Para ver los Taints de un Nodo se puede utilizar el comando:

```
kubectl describe node kubemaster | grep Taint
```

### Tolerations

La definición del Pod se incluye en la sección ```spec```.

```
  tolerations:
    - key: "app"
      operator: "Equal"
      value: "blue"
      effect: NoSchedule
```

### Node Selectors

Definición:

```
kubectl label nodes <node-name> <label-key>=<label-value>
```

```
kubectl label nodes node1 size=Large
```

Un Pod tiene esta definición en la sección ```spec```

```
  nodeSelector:
    size: Large
```

### Node Affinity

```
  affinity:
    nodeAffinity:
      requiredDuringSchedulingIgnoredDuringExecution:
        nodeSelectorTerms:
          - matchExpressions:
            - key: size
              operator: In
              values:
                - Large
```

## Observability

### Readiness Probe

Sirve para que Kubernetes sepa si el Pod está listo para recibir tráfico. Puede pasar que tengamos un Pod corriendo, pero no está listo para recibir tráfico porque nuestra aplicación necesita conectarse primero a la base de datos, por ejemplo.

* Definición

```
    readinessProbe:
        httpGet:
          path: /ready
          port: 8080
        initialDelaySeconds: 10
        periodSeconds: 5
        failureThreshold: 8
```

### Liveness Probe

Sirve para que Kubernetes sepa si el Pod está vivo, es decir, si la aplicación está corriendo o no. Hay aplicaciones que tardan en iniciar (aplicaciones en Java por ejemplo) y el puerto aun no responde, entonces necesita un tiempo para iniciar. Kubernetes restartea el Pod cuando no está vivo.

Hay 3 formas:

* HTTP Test

```
    livenessProbe:
        httpGet:
          path: /health
          port: 8080
        initialDelaySeconds: 1
```

* TCP Test

```
    livenessProbe:
      tcpSocket:
        port: 8080
      initialDelaySeconds: 15
      periodSeconds: 20
```


* Ejecutar comando

```
    livenessProbe:
      exec:
        command:
        - cat
        - /tmp/healthy
      initialDelaySeconds: 5
      periodSeconds: 5
```

### Monitoring

Algunos comandos útiles para monitorear.

```
kubectl top node
```

```
kubectl top pod
```