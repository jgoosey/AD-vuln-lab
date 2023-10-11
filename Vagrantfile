Vagrant.configure("2") do |config|
  
# virtualbox provider by default; swap the comment to change to vmware_desktop
ENV['VAGRANT_DEFAULT_PROVIDER'] = 'virtualbox'
# ENV['VAGRANT_DEFAULT_PROVIDER'] = 'vmware_desktop'

boxes = [
  # if you would like to use vmware, you'll need to use StefanSherer vagrant boxes (https://app.vagrantup.com/StefanScherer) or others that have a vmware_workstation version
  # windows server 2016
  { :name => "DC01",  :ip => "192.168.56.10", :box => "jborean93/WindowsServer2016", :os => "windows"},
  # windows server 2012r2
  { :name => "DC02",  :ip => "192.168.56.11", :box => "jborean93/WindowsServer2012R2", :os => "windows"},
  # windows server 2016
  { :name => "WKST01", :ip => "192.168.56.20", :box => "gusztavvargadr/windows-10", :os => "windows"}#, # uncomment comma to use ELK
  # ELK
  #{ :name => "elk", :ip => "192.168.56.50", :box => "bento/ubuntu-18.04", :os => "linux",
  #  :forwarded_port => [
  #    {:guest => 22, :host => 2210, :id => "ssh"}
  #  ]
  #}
]
  
  config.vm.provider "virtualbox" do |v|
    v.memory = 2048
    v.cpus = 2
  end

  config.vm.provider "vmware_desktop" do |v|
    v.vmx["memsize"] = "4000"
    v.vmx["numvcpus"] = "2"
  end

  config.vm.boot_timeout = 600
  config.vm.graceful_halt_timeout = 600
  config.winrm.retry_limit = 30
  config.winrm.retry_delay = 10

  boxes.each do |box|
    config.vm.define box[:name] do |target|
      # BOX
      target.vm.provider "virtualbox" do |v|
        v.name = box[:name]
        v.customize ["modifyvm", :id, "--groups", "/LAB"]
      end
      target.vm.box_download_insecure = box[:box]
      target.vm.box = box[:box]
      if box.has_key?(:box_version)
        target.vm.box_version = box[:box_version]
      end

      # IP
      target.vm.network :private_network, ip: box[:ip]

      # OS specific
      if box[:os] == "windows"
        target.vm.guest = :windows
        target.vm.communicator = "winrm"
        target.vm.provision :shell, :path => "extras/Install-WMF3Hotfix.ps1", privileged: false
        target.vm.provision :shell, :path => "extras/ConfigureRemotingForAnsible.ps1", privileged: false

        # fix ip for vmware
        if ENV['VAGRANT_DEFAULT_PROVIDER'] == "vmware_desktop"
          target.vm.provision :shell, :path => "extras/fix_ip.ps1", privileged: false, args: box[:ip]
        end

      else
        target.vm.communicator = "ssh"
      end

      if box.has_key?(:forwarded_port)
        # forwarded port explicit
        box[:forwarded_port] do |forwarded_port|
          target.vm.network :forwarded_port, guest: forwarded_port[:guest], host: forwarded_port[:host], host_ip: "127.0.0.1", id: forwarded_port[:id]
        end
      end

    end
  end

end