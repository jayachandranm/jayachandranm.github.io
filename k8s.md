* TOC
{:toc}

## Kubernetes CLI ref.

### General
run master (minikube) first.  
minikube start (--vm-driver=virtualbox)  
kubectl version  
kubectl cluster-info  
kubectl get nodes  
kubectl label nodes 192.168.99.100 shouldrun=here  
kubectl run dep_name --image={docker_image} --port=8080  
kubectl get deployments  
kubectl describe deployment (deploy)  
kubectl delete deployment dep_name  
kubectl get pods  
kubectl get pods --show-labels  
kubectl get pods --output=wide  
kubectl describe pod pod_name  
kubectl describe pod_name  
kubectl logs pod_name  
kubectl logs --tail=5 pod_name -c container_name  
kubectl logs -f --since=10s pod_name -c container_name  
kubectl logs -f -c ruby web-1  
kubectl logs -f deployment/drupal  
kubectl exec pod_name cmd  
kubectl exec -it shell-demo -- /bin/bash  
login to the EC2 machine like normal. If AWS, use admin user for debian user used by kops.
kubectl proxy --port=8080  
### Service
kubectl get services (svc)  
kubectl expose deployment/kubernetes-bootcamp --type="NodePort" --port 8080 (same port as used in run)  
kubectl expose deployment/my-nginx service "my-nginx" exposed  
kubectl get services/kubernetes-bootcamp -o go-template='{{(index .spec.ports 0).nodePort}}'  
sudo iptables-save | grep simpleservice  
kubectl label pod $POD_NAME app=v1  
kubectl get pods -l app=v1  
kubectl delete service -l (--selector) run=kubernetes-bootcamp  
kubectl get pods --selector owner=michael  
kubectl get pods -l 'env in (production, development)'  
### Rolling Updates
kubectl scale deployments/kubernetes-bootcamp --replicas=4  
kubectl set image deployments/kubernetes-bootcamp kubernetes-bootcamp=jocatalin/kubernetes-bootcamp:v2  
kubectl rollout undo deployments/kubernetes-bootcamp  
kubectl rollout status deploy/sise-deploy  
kubectl rollout history deploy/sise-deploy  
kubectl rollout status deployment/nginx-deployment  
### Config
kubectl config use-context minikube  
kubectl get events  
kubectl config view  
### Controllers  
kubectl get rc  
kubectl get rs  
kubectl get ns  
kubectl get pods --namespace=test  
kubectl create secret generic apikey --from-file=./apikey.txt  
kubectl describe secrets/apikey  
### Jobs
kubectl get jobs  
kubectl describe jobs/countdown  
kubectl delete job countdown  
### EP, Labels  
kubectl get ep my-nginx  
environment = production  
environment in (production, qa)  
kubectl get pods -l environment=production,tier=frontend  
kubectl get pods -l 'environment in (production),tier in (frontend)'  
### Rolling updates
kubectl delete pvc mysql-pv-claim  
$pods=$(kubectl get pods --selector=app=nginx --output=jsonpath={.items..metadata.name})  
$echo $pods  
kubectl get rs  
kubectl edit deployment/nginx-deployment  
kubectl rollout history deployment/nginx-deployment (--record while deploying will show change reason)  
kubectl rollout history deployment/nginx-deployment (--revision=1)  
kubectl rollout undo deployment/nginx-deployment (--to-revision=1)  
kubectl scale deployment nginx-deployment --replicas=10  
kubectl autoscale deployment nginx-deployment --min=10 --max=15 --cpu-percent=80  
kubectl rollout pause deployment/nginx-deployment  
kubectl set image deploy/nginx-deployment nginx=nginx:1.9.1  
kubectl rollout resume deploy/nginx-deployment  
kubectl get rs -w  
kubectl patch deployment/nginx-deployment -p '{"spec":{"progressDeadlineSeconds":600}}'  
kubectl get deployment nginx-deployment -o yaml  
### Cleanup
kubectl delete jobs/pi  
kubectl delete -f ./job.yaml  
kubectl get cronjob hello  
kubectl get jobs --watch  
kubectl delete cronjob hello  
kubectl get pod -o go-template='{{range.status.containerStatuses}}{{"Container Name: "}}{{.name}}{{"\r\nLastState: "}}{{.lastState}}{{end}}' simmemleak-60xbc  
kubectl taint nodes node1 key=value:NoSchedule  
### Secrets  
kubectl create secret generic db-user-pass --from-file=./username.txt --from-file=./password.txt  
kubectl get secrets  
kubectl describe secrets/db-user-pass  
kubectl create -f ./secret.yaml  
kubectl get secret mysecret -o yaml  
echo "MWYyZDFlMmU2N2Rm" | base64 --decode  
kubectl create secret generic ssh-key-secret --from-file=ssh-privatekey=/path/to/.ssh/id_rsa --from-file=ssh-publickey=/path/to/.ssh/id_rsa.pub  

kubectl config view  
### Namespace
kubectl get pods --namespace=kube-system -l k8s-app=kube-dns  
kubectl logs --namespace=kube-system $(kubectl get pods --namespace=kube-system -l k8s-app=kube-dns -o name) -c kubedns  
kubectl get svc --namespace=kube-system  
kubectl get ep kube-dns --namespace=kube-system  
kubectl get svc my-nginx -o yaml | grep nodePort -C 5  
kubectl edit svc my-nginx  

Federated Services  

kubectl get ing  

kubectl apply -f hostaliases-pod.yaml  

kubectl create -f docs/user-guide/nginx/nginx-svc.yaml -f docs/user-guide/nginx/nginx-deployment.yaml  
kubectl create -f docs/user-guide/nginx/ (use all yaml, yml json files in the directory)  
kubectl delete -f docs/user-guide/nginx/  
kubectl delete deployments/my-nginx services/my-nginx-svc  
kubectl delete deployment,services -l app=nginx  
kubectl get $(kubectl create -f docs/user-guide/nginx/ -o name | grep service)  
kubectl create -f project/k8s/development --recursive  
kubectl get pods -Lapp -Ltier -Lrole  
kubectl label pods -l app=nginx tier=fe (apply fe label)  
kubectl get pods -l app=nginx -L tier  
kubectl annotate pods my-nginx-v4-9gw19 description='my frontend running nginx'  
kubectl get pods my-nginx-v4-9gw19 -o yaml  
kubectl autoscale deployment/my-nginx --min=1 --max=3  

kubectl apply -f docs/user-guide/nginx/nginx-deployment.yaml (version controlled config to cluster)  
kubectl edit deployment/my-nginx (= get+edit+apply)  
(specify the editor with your EDITOR or KUBE_EDITOR environment variables)  
kubectl patch  
kubectl replace -f docs/user-guide/nginx/nginx-deployment.yaml --force  
kubectl run my-nginx --image=nginx:1.7.9 --replicas=3  
kubectl edit deployment/my-nginx  

kubectl logs --previous  
kubectl logs counter count-log-1  

sudo sysctl -a  

kubectl create namespace myspace  
kubectl create -f ./compute-resources.yaml --namespace=myspace  
kubectl create -f ./object-counts.yaml --namespace=myspace  
kubectl get quota --namespace=myspace  
kubectl describe quota compute-resources --namespace=myspace  
kubectl describe quota object-counts --namespace=myspace  
kubectl create -f ./psp.yaml  
kubectl get psp  
kubectl edit psp permissive  
kubectl delete psp permissive  

### Wordpress example  
kubectl create -f local-volumes.yaml  
kubectl get pv  
kubectl create secret generic mysql-pass --from-literal=password=YOUR_PASSWORD  
kubectl get secrets  
kubectl create -f mysql-deployment.yaml  
kubectl get pods  
kubectl create -f wordpress-deployment.yaml  
kubectl get services wordpress  
minikube service wordpress --url  
kubectl delete secret mysql-pass  
kubectl delete deployment -l app=wordpress  
kubectl delete service -l app=wordpress  
kubectl delete pvc -l app=wordpress  
kubectl delete pv local-pv-1 local-pv-2  

### Setup
minikube start  
(Minikube VM boots into a tmpfs)  
(Minikube is configured to persist files stored under the following host directories)  
ImagePullSecrets  
eval $(minikube docker-env)  
docker ps  
minikube get-k8s-versions  
minikube start --kubernetes-version v1.7.3  
minikube dashboard  
minikube service [-n NAMESPACE] [--url] NAME  
minikube ip  
kubectl get service $SERVICE --output='jsonpath="{.spec.ports[0].nodePort}"'  
minikube start --docker-env HTTP_PROXY=http://$YOURPROXY:PORT  
minikube stop  
minikube delete  

kubeadm - create custom cluster from scratch  
kubectl api-versions  
kubeadm - create custom cluster from scratch  
kubectl api-versions  
  
### StatefulSet
for i in 0 1; do kubectl exec web-$i -- sh -c 'echo $(hostname) > /usr/share/nginx/html/index.html'; done  
for i in 0 1; do kubectl exec -it web-$i -- curl localhost; done  
kubectl delete statefulsets web  
kubectl delete pod -l app=nginx  
kubectl get pods -w -l app=nginx  
kubectl scale sts web --replicas=5  
kubectl patch sts web -p '{"spec":{"replicas":3}}'  
kubectl patch statefulset web -p '{"spec":{"updateStrategy":{"type":"RollingUpdate"}}}'  

kubectl cordon $MYSQL_NODE  
kubectl uncordon $MYSQL_NODE  

kubectl get hpa  

 
kubectl cordon $MYSQL_NODE  
kubectl uncordon $MYSQL_NODE  
  
kubectl get hpa  

### Ingress
apply for cert for *.dev.example.com  
edit ingress-controller.yaml to add cert and handle https, use latest version of nginx-ingress (provider, google)  
create default-backend-service  
kubectl create -f ingress-controller.yaml (nginx deployment and service with ELB)  
from AWS Route53 create new domain record for *.dev.example.com as CNAME for the nginx-ingress ELB.  
edit ingress.yaml with subdomains foo.dev.example.com, bar.dev.example.com pointing to different services.  
use the annotations to force http to https redirect.  
use the wordpress example for k8s (above), change type from LoadBalancer to NodePort  
create mysql and wordpress deployments  
create an echoserver service as second service  
kubectl -f create ingress.yaml  
browse https://foo.dev.example.com, https://bar.example.com  
observe logs, kubectl logs -f deployment/nginx-ingress  

kubectl create -f https://raw.githubusercontent.com/kubernetes/kops/master/addons/kubernetes-dashboard/v1.5.0.yaml  
kubectl apply -f https://raw.githubusercontent.com/kubernetes/dashboard/master/src/deploy/recommended/kubernetes-dashboard.yaml  
browse https://api.prod-cluster-1.blugraph.services/api/v1/namespaces/kube-system/services/kubernetes-dashboard/proxy  
get password for admin from, kubectl config view —minify  
https://api.prod-cluster-1.blugraph.services/ui (with v1.8.0)  
select token and just enter the password (kubectl config view —minify)  
Heapster has to be running in the cluster for the metrics and graphs to be available.  
kubectl apply -f https://raw.githubusercontent.com/kubernetes/kops/master/addons/monitoring-standalone/v1.7.0.yaml  
refresh the dashboard page, the CPU, memory matrix will appear after a while  

### aob  
http://blog.kubernetes.io/2016/08/security-best-practices-kubernetes-deployment.html  
Resources created in one namespace can be hidden from other namespaces.  
Network segmentation is important to ensure that containers can communicate only with those they are supposed to.   
The standard output and standard error output of each container can be ingested using a Fluentd agent running on each node into either Google Stackdriver Logging or into Elasticsearch and viewed with Kibana.  

