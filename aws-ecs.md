## AWS ECS

Storage Configuration on EC2 instance,  
[ec2-user]$ sudo vgs  
[ec2-user]$ sudo lvs  
[ec2-user]$ docker info | grep "Data Space"  

SSH bastion instance  

http://docs.aws.amazon.com/AmazonECS/latest/developerguide/application_architecture.html ??  

(1)  
116.12.201.6/32 (default: Source for MySQL)  
(Change this to 0.0.0.0/0, otherwise not able to access from the ECS Instance)  

(2)  
Database had to be recreated in new VPC. Probably some connectivity was lost while trying to delete the cluster.  

(3)  
ELB registration fails if the website itself is not running (expecting a 200 OK reply).   

(4)  
Setting user to www-data in task caused failure on port creation for container, only root or default user is able to create port 80.  

(5)  
Adding a new instance to existing cluster did not work. Adding a new cluster to existing VPC also failed (probably VPC incomplete?). Cluster name is provided as user data for ECS Instance.  
https://github.com/hashicorp/terraform/issues/5660  

(6)  
Use separate Security groups for ELB and ECS Instances. Ephemeral port connection must be configured between these security groups (inbound for ECS Instance) to allow dynamic port mapping.  

(7)  
Take care of module db configuration.  

To remove a service,  
Click to update the service and set number of tasks to 0. Update the service and then delete.  
https://gist.github.com/dasgoll/0499e785a5ae068b6659  
