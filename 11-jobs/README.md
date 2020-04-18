## Jobs

Los Jobs tienen como objetivo asegurar que los Pods cumplan una tarea exitosamente.

La definición de un Job tiene como particularidad que la sección ```template``` se incluye la sección ```spec``` del Pod que se desea ejecutar.

* Ejemplo Pod:

```
apiVersion: v1
kind: Pod
metadata:
  name: math-pod
  labels:
    name: math-pod
spec:
  containers:
  - name: math-add
    image: ubuntu
    command: ['expr', '3','+','2']
  restartPolicy: Always
```

* Ejemplo Job:
```
apiVersion: batch/v1
kind: Job
metadata:
  name: math-add-job
spec:
 template:
  spec:
    containers:
    - name: math-add
      image: ubuntu
      command: ['expr', '3','+','2']
    restartPolicy: Never
```

## Multiple Pods
Es normal y común que los Jobs tengan más de un Pod asociados, para estos casos existe un property llamado ```completions```.

``` 
apiVersion: batch/v1
kind: Job
metadata:
  name: math-add-job
spec:
 completions: 3
``` 

Los Pods son creados una vez que uno finalice su tarea exitosamente. Si uno falla, volverña a crear un nuevo Pod hasta que llegue al objetivo deseado. Si se desea ejecutar un Job en paralelo, se debe agregar el property parallelism.


## CronJobs

Un CronJob corresponde a un Job con una calendarización. Se suele usar para la generación de reportes o envío de emails, por ejemplo.

En la definición, la sección ```spec``` del Job se incluye en la sección ```jobTemplate``` del CronJob.


