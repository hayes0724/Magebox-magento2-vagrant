# -*- mode: ruby -*-
# vi: set ft=ruby :

# Basic Config
guest_magento_dir = "/home/vagrant/mage2" # Where Magento will be installed on guest OS

guest_magento_app_dir = "/home/vagrant/mage2/public_html/app/"

# Auto installer method - Auto install magento2 with configuration in env.sh
auto_installer_enabled = true

module OS
    def OS.is_windows
        (/cygwin|mswin|mingw|bccwin|wince|emx/ =~ RUBY_PLATFORM) != nil
    end
end

use_nfs_for_synced_folders = !OS.is_windows

VAGRANTFILE_API_VERSION = "2"


Vagrant.configure(VAGRANTFILE_API_VERSION) do |config|
  # All Vagrant configuration is done here. The most common configuration
  # options are documented and commented below. For a complete reference,
  # please see the online documentation at vagrantup.com.

  # box name.  Any box from vagrant share or a box from a custom URL.
  config.vm.box = "ubuntu/trusty64"
  config.ssh.username = "vagrant"
  config.ssh.password = "vagrant"
  config.ssh.private_key_path = "~/.ssh/id_rsa"
  config.ssh.forward_agent = true

  # box modifications, including memory limits and box name.
  config.vm.provider "virtualbox" do |vb|
     vb.name = "mage2"
     vb.memory = 4096
	   vb.cpus = 2
  end

  ## IP to access box
  config.vm.network "private_network", ip: "192.168.33.10"

  # If you want to share an additional folder, (such as a project root).
  # If you experience slow throughput or performance on the folder share, you
  # might have to use an OS specific share.
  #
  # Default Share:
  # config.vm.synced_folder "../data", "/vagrant_data"
  #
  # NFS Share, Good for Unix based OS (Mac OSX, Linux):
  # config.vm.synced_folder ".", "/vagrant", type: "nfs"
  #
  # SMB Share, good for Windows:
  # (If experiencing issues, upgrade PowerShell to V 3.0)
  # config.vm.synced_folder ".", "/vagrant", type: "smb"



  if use_nfs_for_synced_folders
      config.vm.synced_folder "mage2", guest_magento_dir, type: "nfs", create: true
      config.vm.synced_folder "scripts", "/home/vagrant/scripts", type: "nfs", create: true
  else
      config.vm.synced_folder "share", "/home/vagrant/share"
      #config.vm.synced_folder "app", guest_magento_app_dir, type: "smb", create: true
  end

  # Bootstrap VM with software that needs sudo
  config.vm.provision "shell" do |s|
    s.path = "share/scripts/magebox"
    s.args = "bootstrap"
  end

  # Bootstrap VM with software that needs to be installed as web user
  if auto_installer_enabled
    config.vm.provision "shell" do |s|
      s.path = "share/scripts/magebox"
      s.args = "install"
      s.privileged = false
    end
  end


  # If you need to forward ports, you can use this command:
  # config.vm.network "forwarded_port", guest: 80, host: 8080
end
