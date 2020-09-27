## Servicios en Kubernetes

### intro

	> Que es un servicio
	> Tipo de servicios
	> Gestionar y configurar servicios
	> Rolling Updates
	> Rollbacks
	> Recreate

### Clase 1

	Servicios. Componente intermedio que permite la interacción entre el cliente y el cluster.

	El servicio lo que hace es mandar la peticion del cliente a cada pod disponible. Permite tener un ip fijo de conexión.

	Tipos de Servicios.

	> ClusterIP : Accesible solo desde dentro del cluster. Red local del cluster.

	> NodePort : Accesible desde fuera del cluster. Puerto externo.

	> LoadBalancer: Accesible desde fuera. Requiere ser integrado por un servicio de terceros. Es lo mas parecido al nodePort.

	> ExternalName:

	El servicio utiliza selectors para mantener una conexión con los pods.

### Clase 2

	Crear un servicio.

	Vamos a desplegar un servicio, paso a paso.

	Recursos para esta practica en /Deployments/practica_servicio

	Lo primero que vamos a hacer es crear una imagen a partir del Dockerfile

	> docker build -t web .

	Para comprobar que todo funciona correctamente vamos a probar la imagen construyendo el contenedor.

	> docker run -d -p 80:80 web

	Podemos comprobar el funcionamiento en localhost:80

### Clase 3

	Una vez que tengamos la imagen lista. Pasamos al apartado de kubernetes.

	Vamos a desplegar los contenedores en el cluster. Para eso ejecutamos el fichero yaml.

	> kubectl create -f deploy_web.yaml

	Una vez que los pods esten corriendo podemos comprobar que todo vaya bien entrando a uno de los pods.

	> kubectl exec -it [pod-name] bash

	> curl localhost

	Ahora bien, si vamos a nuestro navegador y accedemos a localhost:80 podemos ver que no existe ninguna conexión.

	Por lo que vamos a exponer los pods por medio de un servicio.

	> kubectl expose deployment web-d --name=web-svc --target-port=80 --type=NodePort

	De esta manera, exponemos nuestro servicio por el puerto 80. El tipo de servicio sera NodePort.

	> kubectl get service / svc

	> kubectl describe svc web-svc

	Como trabajamos con Minikube, utilizamos la ip del nodo de minikube.

	> minikube ip

	Con esta ip vamos a nuestro navegador y mapeamos el puerto del node port

	> $ip:32165 (en mi caso/ puede variar)

	Esto nos mostrara la pagina que esta corriendo en apache, de igual manera que vimos en el contenedor de docker.

### Clase 4

	Servicio en modo declarativo.

	Recursos en Deployments/practica_servicio2

	En este caso vamos a crear un servicio pero de manera declarativa. Que es algo recomendable.

	Lo que hacemos es aplicar el archivo web-svc.yaml

	> kubectl apply -f web-svc.yaml

	De igual manera podemos provar que todo funciona correctamente. Consultado el ip de minikube y el puerto del node Port.

### Clase 5

	Rolling Updates. Va modificando los pods de manera evolutiva. De esta manera no corto el servicio. Van cambiando de a uno.

	Recreate. Destruye todo y vuelve a crear los pods nuevos de cero. Por lo que habra una caida de servicio.

	Recursos Deployments/deploy_nginx2.yaml

	> kubectl create -f deploy_nginx2.yaml

	El comando va desplegar los pods en el cluster.

	Ahora lo que queremos hacer es una modificacion en el deployment para poder apreciar el cambio de pods.

	Un cambio considerable puede ser cambiar la imagen del contenedor. O inclusive la version de la imagen.

	> kubectl apply -f deploy_nginx2.yaml

	> kubectl get pods

	Podemos ver como de a poco iran terminando algunos, seguiran corriendo otros y estaran iniciando los nuevos pods con la nueva imagen.

	Esto evita la perdida de servicio.

### Clase 6

	En este caso vamos agregar una propiedad mas al rollingUpdate.

	Recursos /Deployments/deploy_nginx3.yaml nginx3-svc.yaml

	> maxUnavailable: Nunca mas de (n+1) El 1 puede variar

	> maxSurge: Nunca menos de (n-1) El 1 puede variar

	> minReadySeconds : Tiempo minimo para correr los cambios.

	De igual manera podemos generar un cambio en el deployment y ver como reaccionan los pods al cambio.

	Con el siguiente comando podemos ver un historial de rollout. Y con el flag --revision vemos en detalle lo que hay desplegado.

	> kubectl rollout history deploy [name-deploy]

	> kubectl rollout history deploy [name] --revision=[número]

### Clase 7

	Rollback.

	En el caso de que hayamos hecho un rollingUpdate y no estemos de acuerdo con el cambio, intentaremos volver a una version anterior.

	> kubectl rollout history deployment [name]

	> kubectl rollout undo deployment [name] --to-revision=[Número]

	Con rollout undo lo que hacemos es volver a una version anterior.

### Clase 8

	Recreate. Como dijimos anteriormente, es otro tipo de estrategia para actualizar mi deployment.

	Con la diferencia que si aplicamos algun cambio al deployment, este cortara el servicio por completo y lo levantara de nuevo desde cero.

	No es una buena estrategia si queremos mantener nuestro servicio disponible. Puede aplicarse en un problema particular, a forma de reinicio de sistemas.

	strategy:
		type: Recreate
