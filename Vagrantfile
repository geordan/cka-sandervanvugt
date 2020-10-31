IMAGE_NAME = "bento/ubuntu-16.04"
N = 3

Vagrant.configure("2") do |config|
  config.ssh.insert_key = false

  config.vm.provider "virtualbox" do |v|
	v.memory = 1024
	v.cpus = 2
  end

  # Name/IP should match in step "Initialize the Kubernetes cluster using kubeadm"
  # in kubernetes-setup/control-playbook.yml
  config.vm.define "control1" do |master|
	master.vm.box = IMAGE_NAME
	master.vm.network "private_network", ip: "192.168.50.10"
	master.vm.hostname = "control1"
	master.vm.provision "ansible" do |ansible|
	  ansible.playbook = "kubernetes-setup/playbook.yml"
	  ansible.extra_vars = {
		node_ip: "192.168.50.10",
	  }
      ansible.groups = {
        "controllers" => ["control1"]
      }
	end
  end

  (1..N).each do |i|
	config.vm.define "worker#{i}" do |node|
	  node.vm.box = IMAGE_NAME
	  node.vm.network "private_network", ip: "192.168.50.#{i + 10}"
	  node.vm.hostname = "worker#{i}"
	  node.vm.provision "ansible" do |ansible|
		ansible.playbook = "kubernetes-setup/playbook.yml"
		ansible.extra_vars = {
		  node_ip: "192.168.50.#{i + 10}",
		}
        ansible.groups = {
          "workers" => ["worker#{i}"]
        }
	  end
	end
  end
end
