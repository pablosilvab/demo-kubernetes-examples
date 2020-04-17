
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

### Node Affinity

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

