
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