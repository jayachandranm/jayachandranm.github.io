download .deb file from vagrant website and install  
download ngrok binary as zip, unzip and move the binary to /usr/loca/bin  
vagrant plugin install vagrant-share  
cd vagrant-test  
vagrant init  
vagrant box add hashicorp/precise64  
vagrant up  
vagrant ssh  
vagrant destroy  
vagrant box remove hashicorp/precise64  
/vagrant (->/home/{user}/vagant-test)  
vagrant reload --provision  
wget -qO- 127.0.0.1  
vagrant share  
vagrant suspend  
vagrant halt  
vagrant destroy  
vagrant up --provider=vmware_fusion  
vagrant up --provider=aws  
vagrant provision  
config.vm.synced_folder "../data", "/vagrant_data"  
vagrant box list  
vagrant package --base mybox(or UUID) --output /path/to/mybox.box (existing VBox to Vagrant box)  
(https://www.vagrantup.com/docs/cli/package.html)  
vagrant box add --name my-box name-of-the-box.box  
vagrant init my-box  
vagrant up  
vagrant status  
vagrant box remove NAME  
vagrant box prune  

https://www.vagrantup.com/docs/docker/  



Consul -> Etcd   
Vault -> Secrets management  
Inspec  
