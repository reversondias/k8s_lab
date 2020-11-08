Vagrant.configure("2") do |config|
  config.vm.box = "hashicorp/bionic64"

  # Loop for creation of three workers VM
  (1..3).each do |w|
    config.vm.define "worker0#{w}" do |worker|
      worker.vm.network "private_network", ip: "10.20.20.11#{w}", virtualbox__intnet: "k8s_network"
      worker.vm.provider "virtualbox" do |vm|
        vm.name = "worker0#{w}"
        vm.memory = 512
        vm.cpus = 1
      end
    end
  end

  # Loop for creation of three controllers VM
  (1..3).each do |c|
    config.vm.define "controller0#{c}" do |controller|
      controller.vm.network "private_network", ip: "10.20.20.10#{c}", virtualbox__intnet: "k8s_network"
      controller.vm.provider "virtualbox" do |vm|
        vm.name = "controller0#{c}"
        vm.memory = 2048
        vm.cpus = 1
      end
    end
  end

  # Loop for creation of VM with CA, DNS and HA proxy. It'll be use to access de cluster
  config.vm.define "utilities" do |utilities|
    utilities.vm.provider "virtualbox" do |vm|
      vm.name = "utilities"
      vm.memory = 512
      vm.cpus = 1
    end
    utilities.vm.network "forwarded_port", guest: 443, host: 6443
    utilities.vm.network "private_network", ip: "10.20.20.100", virtualbox__intnet: "k8s_network"
    utilities.vm.provision "ansible_local" do |ansible|
      ansible.playbook = "provisioning/main_host_playbook.yml"
      #ansible.playbook = "test_playbook.yaml"
      ansible.inventory_path = "inventory"
      ansible.become = true
      ansible.limit = "all"
      ansible.extra_vars = {
        spc_user: "vagrant"
      }
    end
  end
end