## Volumes

Para mantener persistencia de datos en los contenedores que corren en Pods de Kubernetes, se adjuntan volúmenes a éstos una vez que son creados. Como el Pod es transitorio, al adjuntar un Volumen este se mantiene en disco(?) aunque el Pod sea eliminado.

Ejemplo:
```
spec:
  containers:
  - name: alpine
    image: alpine
    command: ["/bin/sh", "-c"]
    args: ["shuf -i 0-100 -n 1 >> /opt/number.out;"]
    volumeMounts:
      - mountPath: /opt
        name: data-volume
  volumes:
    - name: data-volume
      hostPath:
       path: /data
       type: Directory 
```

Al acceder por ssh al cluster, para este tipo de volumen se ha generado un archivo en la ruta ```/data/number.out``` del cluster.