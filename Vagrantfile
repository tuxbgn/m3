$manager_script = <<SCRIPT
echo Swarm Init...
sudo docker swarm init --listen-addr 192.168.99.100:2377 --advertise-addr 192.168.99.100:2377
sudo docker swarm join-token --quiet worker > /vagrant/worker_token
SCRIPT

$worker_script = <<SCRIPT
echo Swarm Join...
sudo docker swarm join --token $(cat /vagrant/worker_token) 192.168.99.100:2377
SCRIPT

Vagrant.configure("2") do |config|

    config.vm.define "docker1" do |docker1|
        docker1.vm.box="shekeriev/debian-11"
        docker1.vm.hostname = "docker1.do1.lab"
        docker1.vm.network "private_network", ip: "192.168.99.100"
        docker1.vm.provision "shell", path: "docker-setup.sh"
        docker1.vm.provision "shell", path: "other-steps.sh"
        docker1.vm.provision "shell", inline: $manager_script
        docker1.vm.provision "shell", path: "deploy-containers.sh" 
        docker1.vm.provider :virtualbox do |vb|
            vb.customize ["modifyvm", :id, "--memory", "1536"]
        end
    end
	
    config.vm.define "docker2" do |docker2|
        docker2.vm.box="shekeriev/debian-11"
        docker2.vm.hostname = "docker2.do1.lab"
        docker2.vm.network "private_network", ip: "192.168.99.101"
        docker2.vm.provision "shell", path: "docker-setup.sh"
        docker2.vm.provision "shell", path: "other-steps.sh"
        docker2.vm.provision "shell", inline: $worker_script
        docker2.vm.provider :virtualbox do |vb|
            vb.customize ["modifyvm", :id, "--memory", "1536"]
        end
    end
	
    config.vm.define "docker3" do |docker3|
        docker3.vm.box="shekeriev/debian-11"
        docker3.vm.hostname = "docker3.do1.lab"
        docker3.vm.network "private_network", ip: "192.168.99.102"
        docker3.vm.provision "shell", path: "docker-setup.sh"
        docker3.vm.provision "shell", path: "other-steps.sh"
        docker3.vm.provision "shell", inline: $worker_script
        docker3.vm.provider :virtualbox do |vb|
            vb.customize ["modifyvm", :id, "--memory", "1536"]
        end
    end
  
  end
  
