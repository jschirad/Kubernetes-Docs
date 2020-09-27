## Deploymets

### Intro

	> Que es un deployment
	> Ques es un workload
	> Crear deployment
	> Gestionar y cofigurar deployments
	> Replicat sets
	> Escalar Deployments
	> Configurar recursos

### Clase 1

	Workloads / Controllers : Los que se encargan de desplegar los contenedores.

	> Pod. La parte mas pequeña de Kubernetes donde se corren los contenedores.

	> Deployment. Componentes que envuelven los pods y que dan mas caracteristicas y estados deseados.

	> Replica set. Encargados del escalado de pods cuando sea necesario.

	> Stateful set. Gestiona el despliegue y el escalado de los pods. Garantiza el orden y unicidad.

	> Deamon set. Componente que se encarga de que cada nodo del cluster tenga un pod.

	> Job. Se asegura que los pod inicien o terminen correctamente.

	> Cron job. Planificar estados deseados.

	Estos workloads estan asociados con los controllers. Estos son componentes de tipo loop que se encargan que el cluster se encuentre en el estado deseado.


### Clase 2

	Deployment.

	Componente que contiene ciertas caracteristicas para escalar, updates, rollbacks. Este envuelve al pod con estas caracteristicas.
	Practicamente el deployment se encarga de mantener a los pod corriendo. Gestiona los replicaset.
	Es la unidad de trabajo mas habitual para manejar kubernetes.

### Clase 3

	Deployment modo imperativo.

	> kubectl create deployment apache --image=httpd

	Este comando lo que hace es crear un pod, el propio deployment y un replicaset asociado.

	> kubectl describe [deploy-name]

	Con este comando podemos ver las caracteristicas del deployment.

	> kubectl get deploy [deploy-name] -o yaml

	Con este comando podemos ver las caracteristicas del deployment en version yaml.

### Clase 4

	Deployment modo declarativo.

	Ver /Deployments.

	> kubectl create -f [fichero.yaml]

	> kubectl apply -f [fichero.yaml]

	> kubectl edit deploy [deploy-name]

	> kubectl get deployment

	> kubectl get pod

### Clase 5

	Escalar un deployment.

	> kubectl scale deploy [name-deploy] --replicas=[numero]

	Si dentro de el fichero yaml. Configuramos una label, como por ejemplo > estado = "1" y seguido de eso ejecutamos

	> kubectl scale deploy -l estado=1 --replicas=10

	Lo que estamos indicando es que los deployments que contengan la etiqueta (estado = "1") escalen a 10.

### Clase 6

	Memoria de un deployment.

	Vamos a configurar la memeria que podra tener un pod.

	Lo que hacemos es agregar la etiquete < resources > en el apartado de contenedores dentro del archivo yaml.

	Aqui configurarmos la memoria con la que inicia el pod (request : memory :) y la memoria limite que puede alcanzar el pod (limits: memory:)

### Clase 7

	CPU de un deployment.

	> kubectl describe node

	Con este comando podemos ver las caracteristicas del nodo con el que estamos trabajando.

	Ademas podemos ver la capacidad que podemos reclamar al nodo.

	Dentro de la etiqueta resources, utilizada anteriormente, podemos agregar la etiqueta (> cpu:) para aplicar cambios.

### Clase 8

	Metrics-Server. Api para medir rendimiento del cluster.

	https://github.com/kubernetes-sigs/metrics-server.git

	> kubectl apply -f /kubernetes

	Si no se activa solo :

	> minikube addons enable metrics-server

	> kubectl top node / pods

	Simula el comando top pero enfocado en el nodo del cluster.
