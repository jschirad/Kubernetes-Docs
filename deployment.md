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
