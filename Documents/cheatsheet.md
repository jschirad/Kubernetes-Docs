# Ejemplo de desplegar Pod/servicio/ingress/LoadBalancer

https://kubernetes.github.io/ingress-nginx/deploy/#using-helm
https://matthewpalmer.net/kubernetes-app-developer/articles/kubernetes-ingress-guide-nginx-example.html
https://kubernetes.github.io/ingress-nginx/examples/auth/basic/

https://docs.oracle.com/en-us/iaas/Content/ContEng/Tasks/contengsettingupingresscontroller.htm
https://github.com/kubernetes/ingress-nginx/blob/master/docs/user-guide/nginx-configuration/annotations.md

https://webcache.googleusercontent.com/search?q=cache:Qz02raDzhn8J:https://www.opsmx.com/blog/implementation-of-basic-authentication-for-prometheus-and-alertmanager/+&cd=1&hl=es&ct=clnk&gl=es
## Desplegar el servicio de ingress-nginx / LoadBalancer

helm repo add ingress-nginx https://kubernetes.github.io/ingress-nginx
helm repo update

helm install ingress-nginx ingress-nginx/ingress-nginx

## Desplegamos pods / servicios

------------------------------------------------------------------------------------

>
kind: Pod
apiVersion: v1
metadata:
  name: apple-app
  labels:
    app: apple
spec:
  containers:
    - name: apple-app
      image: hashicorp/http-echo
      args:
        - "-text=apple"

---

kind: Service
apiVersion: v1
metadata:
  name: apple-service
spec:
  selector:
    app: apple
  ports:
    - port: 5678 # Default port for image
>


-------------------------------------------------------------------------------


kind: Pod
apiVersion: v1
metadata:
  name: banana-app
  labels:
    app: banana
spec:
  containers:
    - name: banana-app
      image: hashicorp/http-echo
      args:
        - "-text=banana"

---

kind: Service
apiVersion: v1
metadata:
  name: banana-service
spec:
  selector:
    app: banana
  ports:
    - port: 5678 # Default port for image

------------------------------------------------------------------------------


## Desplegamos el ingress para acceder a los servicios

apiVersion: extensions/v1beta1
kind: Ingress
metadata:
  name: example-ingress
  annotations:
    ingress.kubernetes.io/rewrite-target: /
spec:
  rules:
  - http:
      paths:
        - path: /apple
          backend:
            serviceName: apple-service
            servicePort: 5678
        - path: /banana
          backend:
            serviceName: banana-service
            servicePort: 5678


----------------------------------------------------------------------------

$ curl -kL http://localhost/apple
apple

$ curl -kL http://localhost/banana
banana