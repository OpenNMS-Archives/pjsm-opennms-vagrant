VAGRANTFILE_API_VERSION = "2"

Vagrant.configure(VAGRANTFILE_API_VERSION) do |config|
  config.vm.box = "opennms-pjsm-centos-6.5"
  config.vm.box_url = "http://mirror.opennms.eu/pub/vagrant/opennms-pjsm/pjsm-opennms-centos-6.5-development.box"

  config.vm.network "forwarded_port", guest: 8980, host: 8980

  config.vm.provider :virtualbox do |vb|
    # Use VBoxManage to customize the VM. For example to change memory:
    vb.customize ["modifyvm", :id, "--memory", "2048"]
    # vb.gui = true
  end

  # This installs chef into the created box
  config.vm.provision :shell do |shell|
    shell.inline = "which chef-client || wget -qO- https://www.opscode.com/chef/install.sh | bash"
  end

  config.vm.provision :chef_solo do |chef|
    chef.json = {
      :opennms => {
        :release => "branches/pjsm-2.0",
        :home => "/opt/opennms",
        :discovery => {
          :foreignsource => "UK-Store-1"
          :range => {
            :start => "10.23.42.1"
            :end => "10.23.42.254"
          },
          :threads => "5"
        }
      },
      :java => {
        :install_flavor => "oracle",
        :jdk_version => "7",
        :oracle_rpm => {
          :type => "jdk"
        },
        :oracle => {
          :accept_oracle_download_terms => "true"
        }
      }
    }
    chef.cookbooks_path = "cookbooks"
    chef.add_recipe "opennms-light"
  end
end
