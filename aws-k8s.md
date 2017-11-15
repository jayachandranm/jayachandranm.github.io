## k8s on AWS based on kops

Install kubectrl  
Download latest stable release of kops binary (kops-linux-amd64) and move to the right path, with exec permissions.  
Install AWS cli tool (sudo pip install awscli)  
Create a new IAM user  
aws configure and enter credentials for the new IAM user.  
Using these credentials create a new group and user kops.  
Set the right policiies and permissions for the kops group.  
Create new credentials for the new kops user.  
aws configure again and update credentials to the kops user.  
Configure DNS, if domain name is available. If kops > 1.6.2, k8s.local can be used instead.  
Create an S3 bucket in us-east-1 region (other regions are not straightforward).  
Set ENV variables for cluster name and S3 bucket for state store (NAME and KOPS_STATE_STORE).   
Select an availability zone,  
aws ec2 describe-availability-zones --region {region}  
Create an SSH key pair. This key pair will be used for SSH into the instances. An existing key pair can also be used.  
ssh-keygen -t rsa (becomes the default key)  
kops create cluster --zones ap-southeast-1a ${NAME}  
kops edit cluster ${NAME} // to change configurations  
Start the cluster, this will take some time,  
kops update cluster ${NAME} --yes  
kops validate cluster (wait for the cluster to form and show ready)  
kubectl get nodes  
kubectl cluster-info  
Various components,  
kubectl -n kube-system get po  

kops delete cluster --name ${NAME} --yes  

### Comfort 
kubectl create -f nfs-server-ebs-pv.yaml  
(No change from GCE version, here it creates an EBS volume with type gp2)  
kubectl create -f nfs-server-rc.yaml  
kubectl create -f nfs-server-service.yaml  
# Get the IP address of NFS server,  
kubectl describe services nfs-server  
edit nfs-pv.yaml and update the IP address  
kubectl create -f nfs-pv.yaml  
kubectl create -f nfs-pvc.yaml  
kubectl create secret generic mysql-pass --from-literal=password=PASS  
edit drupal-deployment.yaml and update mysql host (here we are using RDS)  
kubectl create -f drupal-deployment.yaml  
kubectl get services  
(A load balancer will be created)  
Get the full address of ELB from AWS web.  
