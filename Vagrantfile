$script = <<-'SCRIPT'
sudo apt-add-repository -y ppa:ansible/ansible
sudo apt update
sudo apt install -y ansible
SCRIPT

Vagrant.configure("2") do |config|
  config.vm.box = "hashicorp/bionic64"

  config.vm.provision "shell", inline: $script

  config.vm.define "main" do |main_host|
  	main_host.vm.provision :ansible do |ansible|
  		ansible.playbook = "provisioning/main_host_playbook.yml"
  		ansible.become = true
  	end
  end

  #config.vm.define "db" do |db|
  #  db.vm.box = "mysql"
  #end
end

