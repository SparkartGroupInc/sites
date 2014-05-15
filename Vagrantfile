# Requirements
Vagrant.require_version '>= 1.5.0'
unless Vagrant.has_plugin?('vagrant-solidus-plugin')
  abort "\033[31mVagrant needs an additional plugin for this box, run `vagrant plugin install vagrant-solidus-plugin` and try again\033[0m"
end

Vagrant.configure('2') do |config|
  # Setup Virtual Box
  config.vm.provider "virtualbox" do |box|
    box.name = "Solidus Box"
    box.customize ["modifyvm", :id, "--memory", "1024"]
  end

  # Setup the virtual machine
  config.vm.box = "ubuntu-precise64"
  config.vm.box_url = "http://files.vagrantup.com/precise64.box"
  config.vm.hostname = "solidus-box"
  config.vm.synced_folder ".", "/vagrant", nfs: true

  # TODO: Can't make DHCP work properly (reloading the box results in a "No guest
  # IP was given to the Vagrant core NFS helper." error). So we use a random static
  # IP instead, to hopefully prevent collisions on the network. See:
  #   https://github.com/mitchellh/vagrant/issues/2626
  #   http://docs.vagrantup.com/v2/networking/private_network.html
  config.vm.network :private_network, ip: "192.168.#{rand(256)}.#{rand(256)}"

  # The vagrant-solidus-plugin plugin handles the rest, see:
  #   https://github.com/solidusjs/vagrant-solidus-plugin
  config.vm.provision :solidus
end
