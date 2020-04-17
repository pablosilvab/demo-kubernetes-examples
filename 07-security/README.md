

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