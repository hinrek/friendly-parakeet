# friendly-parakeet

This is based on the Kubernetes examples:
https://kubernetes.io/docs/tutorials/stateless-application/guestbook/

## Prerequisites

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

## Logging EFK Stack

This is based on the Digitalocean examples:
https://www.digitalocean.com/community/tutorials/how-to-set-up-an-elasticsearch-fluentd-and-kibana-efk-logging-stack-on-kubernetes

Create the logging namepsace

```
kubectl create -f logging/kube-logging.yaml
```

Create the Elasticsearch Service

```
kubectl create -f logging/elasticsearch_svc.yaml
```

Create the Elasticsearch StatefulSet

```
kubectl create -f logging/elasticsearch_statefulset.yaml
```

Wait for the Elasticsearch pods to roll out

```
kubectl rollout status sts/es-cluster --namespace=kube-logging
```

Create the kibana stack

```
kubectl create -f logging/kibana.yaml
```

Wait for the Kibana pods to roll out

```
kubectl rollout status deployment/kibana --namespace=kube-logging
```

Get the Kibana pod name (e.g. 'kibana-84cf7f59c-dsjbk')

```
kubectl get pods --namespace=kube-logging
```

Forward the Kibana port

```
kubectl port-forward <kibana pod name> 5601:5601 --namespace=kube-logging
```

Create the fluentd stack

```
kubectl create -f logging/fluentd.yaml
```

If you hava the Kibana port still forwarded navigate to http://localhost:5601 if not forward the port again as we did few steps back.

To be continued... 
