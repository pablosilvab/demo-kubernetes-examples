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

### Monitoring

Algunos comandos útiles para monitorear.

```
kubectl top node
```

```
kubectl top pod
```