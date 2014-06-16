# -*- mode: ruby -*-
# vi: set ft=ruby :

# Marc Rohlfs, Silpion IT-Solutions GmbH - rohlfs@silpion.de
# Liferay Server with MySQL database for local developement
# 2014-01-02

require File.dirname(__FILE__) + '/.vagrant.d/boxes.rb'

#BOX_NAME = 'precise'
BOX_NAME = 'u64server'
BOX = BOXES[BOX_NAME]


Vagrant::Config.run do |config|

  config.vm.box = BOX_NAME
  config.vm.box_url = BOX['uri']

end

Vagrant::VERSION >= '1.1.0' and Vagrant.configure('2') do |config|

  config.vm.hostname = 'liferay-dev'

  if Vagrant.has_plugin?("vagrant-cachier")
    config.cache.auto_detect = true
  end

  config.vm.provision "ansible" do |ansible|
    ansible.playbook = "provisioning/playbook.yml"
    ansible.tags = ENV['TAGS']
  end

  config.vm.synced_folder "liferay-deploy", "/vagrant/liferay-deploy",
    mount_options: ["dmode=777,fmode=777"]

  config.vm.network :forwarded_port, guest: 8080, host: 18080

  config.vm.provider :virtualbox do |vb|
    vb.customize [
      'modifyvm', :id, '--name', 'liferay-dev', '--memory', '2048', '--cpus', '1'
    ]
  end

end
