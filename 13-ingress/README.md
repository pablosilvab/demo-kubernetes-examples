## Ingress

Un Ingress ayuda a los usuarios externos para acceder a la aplicación con una URL que enruta a diferentes servicios dentro del cluster. Podemos ver a un Ingress como una capa Load Balancer construida dentro del cluster, que puede ser ser configurada como un objeto nativo de Kubernetes.

El Ingress permite exponer el servicio fuera del cluster.

### Ingress Controller

No viene instalado por defecto por Kubernetes, se debe desplegar. Una de las soluciones más implementadas es Nginx.

Para instalar Nginx, se deben seguir los pasos que están en la [documentación oficial](https://docs.nginx.com/nginx-ingress-controller/installation/installation-with-manifests/). 

Ejemplo:

```
kubectl apply -f ingress-controller/.
```

### Ingress Resources

Un Ingress Resource son básicamente reglas para dirigir el tráfico, aplicadas sobre el Ingress Controller. Aquí es posible enrutar al usuario dependiendo del recurso solicitado.

Lo anterior se puede definir con un manifiesto Kubernetes denominado ```Ingress```

Ejemplo: 

```
apiVersion: networking.k8s.io/v1beta1
kind: Ingress
metadata:
  name: ingress
  annotations:
    nginx.ingress.kubernetes.io/rewrite-target: /
spec:
  rules:
  - host: v1.helloworld.local
    http:
      paths:
      - backend:
          serviceName: hello-v1-svc
          servicePort: 80
  - host: v2.helloworld.local
    http:
      paths:
      - backend:
          serviceName: hello-v2-svc
          servicePort: 80
```

Prueba: Se utilizan los hosts establecidos en el Ingress y el puerto del Ingress de Nginx.
```
curl v1.helloworld.local:32674
```

```
curl v2.helloworld.local:32674
```

No solo podemos hacer reglas basadas en el ```host``` , también puedes hacer reglas según el ```path```.
