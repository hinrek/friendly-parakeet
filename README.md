# friendly-parakeet

This is based on the Kubernetes examples:
https://kubernetes.io/docs/tutorials/stateless-application/guestbook/

## Prequisits

>NB! Default configuration is using NodePort as the development was done with Minukube.

* Install and Set Up [kubectl]( 
https://kubernetes.io/docs/tasks/tools/install-kubectl/)

* For local development use either Docker desktop with Kubernetes enabled or e.g. [Minukube](https://minikube.sigs.k8s.io/docs/start/) for Linux.


## Deploy the project locally

Redis  

```
kubectl apply -f redis.yaml
```

Frontend 

```
kubectl apply -f frontend.yaml
```

Create a Service Account & ClusterRoleBinding

```
kubectl apply -f dashboard-adminuser.yaml
```

Install Kubernetes Dashboard

```
kubectl apply -f https://raw.githubusercontent.com/kubernetes/dashboard/v2.0.0/aio/deploy/recommended.yaml
```

Start the Dashboard (open in a new terminal window or send the job to background)

```
kubectl proxy
```

Get the Bearer Token for Dashboard login

```
kubectl -n kubernetes-dashboard describe secret $(kubectl -n kubernetes-dashboard get secret | grep admin-user | awk '{print $1}')
```

Paste the token from the previous command into the dashboard login field

[http://localhost:8001/api/v1/namespaces/kubernetes-dashboard/services/https:kubernetes-dashboard:/proxy/](http://localhost:8001/api/v1/namespaces/kubernetes-dashboard/services/https:kubernetes-dashboard:/proxy/)

## Get the application frontend IP & profit


NodePort 

```
minikube service frontend --url
```

LoadBalancer 

```
kubectl get service frontend
```
