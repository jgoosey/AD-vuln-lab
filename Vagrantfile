Vagrant.configure("2") do |config|
  
  config.vm.provider "virtualbox" do |v|
    v.memory = 2048
    v.cpus = 2
    v.check_guest_additions = false
    v.linked_clone = true
    v.gui = false
  end

  config.vm.define "dc" do |dc|
    dc.vm.guest = :windows
    dc.vm.communicator = "winrm"
    dc.vm.boot_timeout = 600
    dc.vm.graceful_halt_timeout = 600
    dc.winrm.retry_limit = 30
    dc.winrm.retry_delay = 10
    dc.vm.box = "jborean93/WindowsServer2016"
    dc.vm.network "private_network", ip: "192.168.56.10"
    dc.vm.network :forwarded_port, guest: 3389, host: 23389, id: "msrdp"
    dc.vm.network :forwarded_port, guest: 5985, host: 25985, id: "winrm"
    dc.vm.provision "shell", path:"ConfigureRemotingForAnsible.ps1"
  end

  config.vm.define "dc2" do |dc2|
    dc2.vm.guest = :windows
    dc2.vm.communicator = "winrm"
    dc2.vm.boot_timeout = 600
    dc2.vm.graceful_halt_timeout = 600
    dc2.winrm.retry_limit = 30
    dc2.winrm.retry_delay = 10
    dc2.vm.box = "jborean93/WindowsServer2012R2"
    dc2.vm.network "private_network", ip: "192.168.56.11"
    dc2.vm.network :forwarded_port, guest: 3389, host: 33389, id: "msrdp"
    dc2.vm.network :forwarded_port, guest: 5985, host: 35985, id: "winrm"
    dc2.vm.provision "shell", path:"ConfigureRemotingForAnsible.ps1"
  end

  config.vm.define "wkst" do |wkst|
    wkst.vm.guest = :windows
    wkst.vm.communicator = "winrm"
    wkst.vm.boot_timeout = 600
    wkst.vm.graceful_halt_timeout = 600
    wkst.winrm.retry_limit = 30
    wkst.winrm.retry_delay = 10
    wkst.vm.box = "gusztavvargadr/windows-10"
    wkst.vm.network "private_network", ip: "192.168.56.20"
    wkst.vm.network :forwarded_port, guest: 3389, host: 43389, id: "msrdp"
    wkst.vm.network :forwarded_port, guest: 5985, host: 45985, id: "winrm"
    wkst.vm.provision "shell", path:"ConfigureRemotingForAnsible.ps1"
  end

  #saving a line for later ELK install
  #config.vm.define "ubuntu_domain" do |ubuntu_domain|
  #  ubuntu_domain.vm.box = "ubuntu/focal64"
  #  ubuntu_domain.vm.network "private_network", ip: "192.168.56.13"
  #  ubuntu_domain.vm.network :forwarded_port, guest: 22, host: 10022, id: "msrdp"
  #end

end