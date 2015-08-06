# -*- mode: ruby -*-
# vi: set ft=ruby :

# Generate a 'random' password, used for internal Tower services
def generate_password(length)
  chars = 'abcdefghjkmnpqrstuvwxyzABCDEFGHJKLMNPQRSTUVWXYZ0123456789'
  password = ''
  length.times { password << chars[rand(chars.size)] }
  password
end

tower_adminpw = ENV['TOWER_ADMINPW'] || 'admin'

tower_config = { 'admin_password'  => tower_adminpw,
                 'munin_password'  => ENV['TOWER_MUNINPW'] || "admin",
                 'database'        => "internal",
                 'primary_machine' => "localhost",
                 'pg_password'     => generate_password(32),
                 'redis_password'  => generate_password(32) }

Vagrant.configure(2) do |config|
  config.vm.box = "boxcutter/centos71"

  # Define Tower machine properties
  config.vm.define "tower", primary:true do |tower|
    # Bump memory/CPU allocation for the Tower instance
    # (NB: Tower has a preflight check that confirms sufficient RAM is
    #      available, don't go below this!)
    tower.vm.provider "vmware_fusion" do |v|
      v.vmx["memsize"] = "2048"
      v.vmx["numvcpus"] = "2"
    end

    # Run Tower installation playbook
    tower.vm.provision :ansible do |ansible|
      ansible.playbook = "installer/install.yml"
      ansible.sudo = true
      ansible.extra_vars = tower_config
    end

    # Register our instance of Tower if we have been given a license file
    if File.exists?('tower-license')
      tower.vm.provision "shell",
        inline: "curl -d @/vagrant/tower-license -f -s -k -X POST -u 'admin:#{tower_adminpw}' -H 'Content-Type: application/json'  https://localhost/api/v1/config/"
    end
  end

  ## Generic Provisioners ##
  # Install pre-reqs
  config.vm.provision "shell", inline: <<-SHELL
    sudo yum -y install epel-release
    sudo yum -y install ansible
  SHELL
end
