## Kubernetes on Google Cloud Engine
Install google-cloud-sdk for Ubuntu  
gcloud init  
gcloud container clusters create hello-world-cluster --zone asia-southeast1-a --machine-type f1-micro  
gcloud container clusters get-credentials example-cluster --zone asia-southeast1-a  
(Updates kubeconfig entry, credentials cached)  
kubectl cluster-info  
kubectl run hello-web --image=gcr.io/google-samples/hello-app:1.0 --port=8080  
kubectl expose deployment hello-web --type="LoadBalancer"  
kubectl get service hello-web  
kubectl delete service hello-web  
gcloud container clusters delete example-cluster --zone asia-southeast1-a  
  
gcloud config set container/cluster NAME  
gcloud container clusters get-credentials NAME  
gcloud auth application-default login  
  
kubectl --context customer-cluster get nodes  
kubectl config view  
kubectl config get-contexts  
kubectl config use-context customer-cluster  
kubectl config set current-context {context-name}  
kubectl config set current-context minikube  
kubectl config use-context default-system  
kubectl config current-context  
  
docker build -t gcr.io/true-server-183802/hello-world-image:v1 .  
gcloud docker -- push gcr.io/true-server-183802/hello-world-image:v1  
kubectl delete service/hello-world-deployment  
gcloud container clusters delete hello-world-cluster --zone asia-southeast1-a  
  
conjure-up provides the quickest way to deploy Kubernetes on Ubuntu for multiple clouds and bare metal  
  
### Wordpress eg
gcloud compute disks create --size 200GB mysql-disk  
kubectl delete service wordpress  
gcloud compute forwarding-rules list (wait for LoadBalancer delete)  
gcloud container clusters delete persistent-disk-tutorial  
gcloud compute disks delete mysql-disk wordpress-disk  
  
### Comfort Drupal
Create GCE_PD for mysql  
gcloud compute disks create --size 200GB mysql-disk --zone asia-southeast1-a  
Create the secret for db root user.  
kubectl create secret generic mysql-pass --from-literal=password=YOUR_PASSWORD  
First deploy the mysql service with LoadBalancer. Then import database from local machine  
mysql -h <EXT_IP> -u root -p  
Create the database, user and grant privileges.  
mysql -h <EXT_IP> -u root -p {my_db} < db_file.sql  
Verify that there are no errors in import.  
Delete the deployment and service. Change mysql service to ClusterIP.   
kubectl create -f mysql-deployment.yaml  
Verify that both service and pods are running.   
Connect to db using another pod in the same cluster and verify that tables are intact.  
kubectl run -it --rm --image=mysql:5.6 --restart=Never mysql-client -- bash  
kubectl run -it --rm --image=mysql:5.6 --restart=Never mysql-client -- mysql -h mysql -u {db_user} -p {db_pass}  
Edit drupal deployment file. Verify secret key.  
Set corrent env for DB access. Set mountpath as /var/www/html/sites/default/files/styles. Mounting operation does not copy files from Image to PV.   
Replicas must be set to 1, as GCE-PD only supports single RW.   
kubectl create -f drupal-deployment.yaml  
kubectl exec -it {pod_name} bash  
Check permissions for html files. Ideally they should be www-data.  
How to scale?  
hostMount -< mounts  a directory.  
pre-populate GCE_PD from outside docker?  

Follow k8s volume/nfs example from github to create NFS mount and then use that with Drupal.  
kubectl create -f nfs-server-gce-pv.yaml  
kubectl create -f nfs-server-rc.yaml  
kubectl create -f nfs-server-service.yaml  
get the cluster IP of the server using the following command  
kubectl describe services nfs-server  
use the NFS server IP to update nfs-pv.yaml and execute the following  
kubectl create -f nfs-pv.yaml  
kubectl create -f nfs-pvc.yaml  
Create secret  
Create MySQL  
Create Drupal  

gcloud container clusters create NAME — zone ZONE — num-nodes=30  — enable-autoscaling — min-nodes=15 — max-nodes=50  
gcloud container clusters upgrade CLUSTER_NAME [ — cluster-version=X.Y.Z]  
