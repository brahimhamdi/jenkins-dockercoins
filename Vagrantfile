Vagrant.require_version ">= 2.0.0"

boxes = [
    {
        :name => "jenkins-ansible-control-plane",
        :eth1 => "192.168.205.100",
        :mem => "4096",
        :cpu => "2"
    },
    {
        :name => "node1",
        :eth1 => "192.168.205.101",
        :mem => "1024",
        :cpu => "1"
    },
    {
        :name => "node2",
        :eth1 => "192.168.205.102",
        :mem => "1024",
        :cpu => "1"
    }
]

Vagrant.configure(2) do |config|
  config.vm.box = "generic/ubuntu2204"

  config.vbguest.auto_update = false if Vagrant.has_plugin?("vagrant-vbguest")

  boxes.each do |opts|
      config.vm.define opts[:name] do |config|
        config.vm.hostname = opts[:name]
        config.vm.provider "virtualbox" do |v|
          v.customize ["modifyvm", :id, "--memory", opts[:mem]]
          v.customize ["modifyvm", :id, "--cpus", opts[:cpu]]
          v.customize ["modifyvm", :id, "--uart1", "0x3F8", "4"]
          v.customize ["modifyvm", :id, "--uartmode1", "file", File::NULL]
        end
        config.vm.network :private_network, ip: opts[:eth1]
      end
  end

  config.vm.provision "shell", inline: <<-SHELL
    sudo apt update
    sudo apt install -y tree git 
    sudo apt install -y ansible

  SHELL

  config.vm.provision "ansible" do |ansible|
    ansible.playbook = "playbooks/install-jenkins.yml"
  end

  config.vm.provision "ansible" do |ansible|
    ansible.playbook = "playbooks/install-docker.yml"
  end
end
