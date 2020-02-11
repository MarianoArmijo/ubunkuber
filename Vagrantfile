# -*- mode: ruby -*-
# vi: set ft=ruby :

VAGRANTFILE_API_VERSION = "2"
Vagrant.require_version ">= 2.0.0"

# All Vagrant configuration is done below. The "2" in Vagrant.configure
# configures the configuration version (we support older styles for
# backwards compatibility). Please don't change it unless you know what
# you're doing.
Vagrant.configure(VAGRANTFILE_API_VERSION) do |config|
  # The most common configuration options are documented and commented below.
  # For a complete reference, please see the online documentation at
  # https://docs.vagrantup.com.

  # Every Vagrant development environment requires a box. You can search for
  # boxes at https://vagrantcloud.com/search.
  config.vm.boot_timeout = 600
  config.ssh.insert_key = false
  config.vm.box = "ubuntu/xenial64"
  config.vm.hostname = "master-node"
  #config.vm.network "public_network", ip: "192.168.1.72", netmask: "24", default: "192.168.1.1", dns: "8.8.8.8"
  #config.vm.network "public_network", type: "dhcp", bridge: "Intel(R) Dual Band Wireless-AC 7265"
  #config.vm.network "public_network", ip: "192.168.1.30", netmask: "24", default: "192.168.1.1", dns: "80.58.61.254", bridge: "Intel(R) Dual Band Wireless-AC 7265"
  config.vm.network "forwarded_port", id: "ssh", host_ip: "127.0.0.1", guest: 22, host: 2221, auto_correct: true
  config.vm.network "forwarded_port", id: "k8s-api", host_ip: "127.0.0.1", guest: 6443, host: 6443, auto_correct: true
  # Disable automatic box update checking. If you disable this, then
  # boxes will only be checked for updates when the user runs
  # `vagrant box outdated`. This is not recommended.
  # config.vm.box_check_update = false

  # Create a forwarded port mapping which allows access to a specific port
  # within the machine from a port on the host machine. In the example below,
  # accessing "localhost:8080" will access port 80 on the guest machine.
  # NOTE: This will enable public access to the opened port
  # config.vm.network "forwarded_port", guest: 80, host: 8080

  # Create a forwarded port mapping which allows access to a specific port
  # within the machine from a port on the host machine and only allow access
  # via 127.0.0.1 to disable public access
  # config.vm.network "forwarded_port", guest: 80, host: 8080, host_ip: "127.0.0.1"

  # Create a private network, which allows host-only access to the machine
  # using a specific IP.
  # config.vm.network "private_network", ip: "192.168.1.88"

  # Create a public network, which generally matched to bridged network.
  # Bridged networks make the machine appear as another physical device on
  # your network.
  # config.vm.network "bridged"

  # Share an additional folder to the guest VM. The first argument is
  # the path on the host to the actual folder. The second argument is
  # the path on the guest to mount the folder. And the optional third
  # argument is a set of non-required options.
  # config.vm.synced_folder "../data", "/vagrant_data"

  # Provider-specific configuration so you can fine-tune various
  # backing providers for Vagrant. These expose provider-specific options.
  # This is for VirtualBox:
  config.vm.provider "virtualbox" do |v|

    # Display the VirtualBox GUI when booting the machine
    v.gui = true
    # Virtual machine name
    v.name = "ubuntu-master-node"
    # Memory
    v.customize ["modifyvm", :id, "--memory", 4096]
    # CPUs
    v.customize ["modifyvm", :id, "--cpus", "4"]
    # Video Ram
    v.customize ["modifyvm", :id, "--vram", "128"]
    # --hwvirtex on|off: This enables or disables the use of hardware virtualization
    # extensions (Intel VT-x or AMD-V) in the processor of your host system;
    v.customize ["modifyvm", :id, "--hwvirtex", "on"]
    # --hpet on|off: This enables/disables a High Precision Event Timer (HPET)
    # which can replace the legacy system timers. This is turned off by default.
    # Note that Windows supports a HPET only from Vista onwards.
    v.customize ["modifyvm", :id, "--hpet", "off"]
    # --pagefusion on|off: Enables/disables (default) the Page Fusion feature.
    # The Page Fusion feature minimises memory duplication between VMs with similar
    # configurations running on the same host. See Section 4.9.2, “Page Fusion” for details.
    v.customize ["modifyvm", :id, "--pagefusion", "on"]
    # --paravirtprovider none|default|legacy|minimal|hyperv|kvm: This setting specifies which
    # paravirtualization interface to provide to the guest operating system.
    v.customize ["modifyvm", :id, "--paravirtprovider", "kvm"]
    # --chipset piix3|ich9: By default VirtualBox emulates an Intel PIIX3 chipset.
    v.customize ["modifyvm", :id, "--chipset", "ich9"]
    v.customize ["setextradata", :id, "CustomVideoMode1", "1024x768x32"]
    v.customize ["modifyvm", :id, "--rtcuseutc", "on"]
    v.customize ["modifyvm", :id, "--clipboard", "bidirectional"]
    v.customize ["modifyvm", :id, "--audio", "none"]
    # Networks
    #v.customize ["modifyvm", :id, "--nic1", "nat"]
    #v.customize ["modifyvm", :id, "--macaddress1", "10304050F1DE"]
  end

  # View the documentation for the provider you are using for more
  # information on available options.

  # Enable provisioning with a shell script. Additional provisioners such as
  # Puppet, Chef, Ansible, Salt, and Docker are also available. Please see the
  # documentation for more information about their specific syntax and use.
  config.vm.provision "file", source: ".vault_pass", destination: "/tmp/.vault_pass"
  config.vm.provision "shell", run: "always", inline: <<-SHELL
    sudo apt-add-repository -y ppa:ansible/ansible && sudo apt update -y && sudo apt install -y ansible \
    && sudo chmod 0600 /tmp/.vault_pass
  SHELL

  #
  # Run Ansible from the Vagrant Host
  #
  config.vm.provision "ansible_local" do |ansible|
    ansible.playbook = "/vagrant/ansible-provision/site.yml"
    ansible.vault_password_file = "/tmp/.vault_pass"
  end
end
