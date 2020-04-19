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

## Persistent Volume

Es posible mantener los volumenes con un manejo centralizado y que los usuarios puedan conectarse a este. 

Un Persistent Volume es un grupo amplio de volúmenes de almacenamiento configurado por un admin para ser usado por los usuarios que hacen deploys. Los usuarios pueden selecciones storage de este pool utilizando PVC.

accessModes: ReadWriteOnce | ReadOnlyMany | ReadWriteMany

Ejemplo:
```
apiVersion: v1
kind: PersistentVolume
metadata:
  name: pv-vol1
spec:
  accessModes:
    - ReadWriteOnce
  capacity:
    storage: 1Gi
  hostPath:
    path: /tmp/data
```

## Persistent Volume Claims (PVC)

El usuario crea un PVC para utilizar el storage del PV. Cuando este es creado, Kubernetes hace el binding del PV con el PVC basado en el request y las properties seteadas en el volumen.

Ejemplo:
```
apiVersion: v1
kind: PersistentVolumeClaim
metadata:
  name: myclaim
spec:
  accessModes:
   - ReadWriteOnce
  resources:
   requests:
     storage: 500Mi
```
