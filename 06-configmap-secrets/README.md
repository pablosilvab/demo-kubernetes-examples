

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