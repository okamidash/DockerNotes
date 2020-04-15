required_plugins = ["vagrant-hostsupdater"] #This ensurs we can change the name of the ip name
required_plugins.each do |plugin|
    exec "vagrant plugin install #{plugin}" unless Vagrant.has_plugin? plugin
end

Vagrant.configure("2") do |config|
  config.vm.define "node1" do |node1|
    node1.vm.box = "ubuntu/bionic64"
    node1.vm.network "private_network", ip: "10.0.10.11"
    node1.hostsupdater.aliases = ["node1.local"]
    node1.vm.synced_folder "ansibles", "/home/vagrant/ansibles/"
    node1.vm.synced_folder "key", "/home/vagrant/key/"
    node1.vm.provision "ansible_local" do |ansible|
      ansible.playbook = "/home/vagrant/ansibles/playbook.yml"
      ansible.inventory_path = "/home/vagrant/ansibles/inventory.yml"
    end
    node1.vm.provider "virtualbox" do |v|
      v.customize ["modifyvm", :id, "--natdnshostresolver1", "on"]
      v.customize ["modifyvm", :id, "--natdnsproxy1", "on"]
    end
  end
end