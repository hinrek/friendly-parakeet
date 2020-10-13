# friendly-parakeet

This is based on the Kubernetes examples: 

https://kubernetes.io/docs/tutorials/stateless-application/guestbook/

https://kubernetes.io/docs/tutorials/stateless-application/guestbook-logs-metrics-with-elk/


## Prequisits


* Install and Set Up [kubectl]( 
https://kubernetes.io/docs/tasks/tools/install-kubectl/)

* For local development use either Docker desktop with Kubernetes enabled or e.g. [Minukube](https://minikube.sigs.k8s.io/docs/start/) for Linux.


## Deploy the project locally


>NB! Default configuration is using NodePort as the development was done with Minukube.

Redis master deployment 

```
kubectl apply -f application/guestbook/redis-master-deployment.yaml
```

Redis master service 

```
kubectl apply -f application/guestbook/redis-master-service.yaml
```

Redis slave deployment 

```
kubectl apply -f application/guestbook/redis-slave-deployment.yaml
```

Redis slave service 

```
kubectl apply -f application/guestbook/redis-slave-service.yaml
```

Frontend deployment 

```
kubectl apply -f application/guestbook/frontend-deployment.yaml
```

Frontend service 

```
kubectl apply -f application/guestbook/frontend-service.yaml
```

Install Kubernetes Dashboard

```
kubectl apply -f https://raw.githubusercontent.com/kubernetes/dashboard/v2.0.0/aio/deploy/recommended.yaml
```

Create a Service Account
```
kubectl apply -f application/guestbook/dashboard-adminuser.yaml
```

Create a ClusterRoleBinding

```
kubectl apply -f application/guestbook/adminuser-cluster-role-binding.yml
```

Get the Bearer Token for Dashboard login

```
kubectl -n kubernetes-dashboard describe secret $(kubectl -n kubernetes-dashboard get secret | grep admin-user | awk '{print $1}')
```

Start the Dashboard

```
kubectl proxy
```

Dashboarb
[http://localhost:8001/api/v1/namespaces/kubernetes-dashboard/services/https:kubernetes-dashboard:/proxy/](http://localhost:8001/api/v1/namespaces/kubernetes-dashboard/services/https:kubernetes-dashboard:/proxy/)

## Get frontend IP


NodePort 

```
minikube service frontend --url
```

LoadBalancer 

```
kubectl get service frontend
```
