## NetworkPolicy

Un recurso NetworkPolicy es una especificación de cómo un grupo de Pods pueden comunicarse entre sí. Se utilizan Labels para seleccionar los Pods y se definen reglas que especifican qué tráfico está permitido hacia los Pods seleccionados.

Ejemplo: 
```
apiVersion: networking.k8s.io/v1
kind: NetworkPolicy
metadata: 
  name: db-policy
spec:
    podSelector:
      matchLabels:
        role: db
    policyTypes:
      - Ingress
    ingress:
    - from:
      - podSelector:
         matchLabels:
           name: api-pod
      ports:
      - protocol: TCP
        port: 3306

```

En el ejemplo, el recurso NetworkPolicy se llama ```db-policy``` y permite ```Ingress Traffic``` desde el Pod con Label ```api-pod``` por el puerto ```3306```.  