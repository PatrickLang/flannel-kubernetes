Vagrant.configure("2") do |config|

  # See https://github.com/hashicorp/vagrant/issues/8796 - workaround for ansible_local
  config.hostmanager.enabled = true
  config.hostmanager.manage_guest = true

  config.vm.define "nodea" do |nodea|
    nodea.vm.box = "bento/ubuntu-16.04"
    nodea.hostmanager.aliases = %w(nodea)
    nodea.vm.provider "hyperv" do |hv|
      hv.vmname = "K8s-nodea"
      hv.memory = 1024
      hv.maxmemory = 2048
      hv.cpus = 2
      hv.linked_clone = true
    end
    nodea.vm.synced_folder ".", "/vagrant", type: "rsync",
      rsync__exclude: ".git/"

    nodea.vm.hostname = "nodea.localdomain"

  end


  config.vm.define "win1" do |win1|
    win1.hostmanager.aliases = %w(win1)
    win1.vm.box = "WindowsServer2019Docker"
    win1.vm.provider "hyperv" do |hv|
      hv.vmname = "K8s-win1"
      hv.memory = 4096
      hv.maxmemory = 8192
      hv.cpus = 2
      hv.linked_clone = true
    end

    win1.vm.hostname = "win1"
  end


  config.vm.define "master" do |master|
    master.hostmanager.aliases = %w(master)
    master.vm.box = "bento/ubuntu-16.04"
    master.vm.provider "hyperv" do |hv|
      hv.vmname = "K8s-master"
      hv.memory = 1024
      hv.maxmemory = 2048
      hv.cpus = 2
      hv.linked_clone = true
    end
    master.vm.synced_folder ".", "/vagrant", type: "rsync",
      rsync__exclude: ".git/"

    master.vm.hostname = "master.localdomain"

    config.vm.provision :hostmanager

    master.vm.provision :ansible_local do |ansible|
      ansible.playbook = "kubernetes-cluster.yml"
      ansible.verbose = true
      ansible.install = true
      # ansible.inventory_path = "inventory"
      ansible.limit = "all"
      ansible.groups = {
	      "kube-master" => ["master"],
	      "kube-minions-linux" => ["nodea"],
	      "kube-minions-windows" => ["win1"],
	      "all_groups:children" => ["kube-master", "kube-minions-linux", "kube-minions-windows"]
      }
    end
  end

end
