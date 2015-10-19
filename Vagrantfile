Vagrant.configure('2') do |config|
  config.omnibus.chef_version = '12.5.1'
  config.vbguest.auto_update = true
  config.vm.box = 'opscode-debian-8.2'
  config.vm.box_url = 'http://opscode-vm-bento.s3.amazonaws.com/vagrant/virtualbox/opscode_debian-8.2_chef-provisionerless.box'
  config.berkshelf.enabled = true
  config.vm.provider :virtualbox do |vb|
    vb.customize ['modifyvm', :id, '--memory', 1024]
  end

  config.vm.network :private_network, ip: '10.10.2.15', netmask: '255.255.255.0'
  config.ssh.forward_agent = true
  config.vm.synced_folder '.', '/vagrant', disabled: false, nfs: true

  config.vm.provision 'chef_solo' do |chef|
    chef.synced_folder_type = 'nfs'

    chef.json = {
      'java' => {
        'jdk_version' => '7'
      },
      'rbenv' => {
        'upgrade' => 'sync',
        'rubies'  => [ '2.2.3' ],
        'global'  => '2.2.3',
        'gems'    => {
          '2.2.3' => [
            {
              'name' => 'bundler',
              'version' => '1.9.9'
            }
          ]
        }
      },
      'redisio' => {
        'package_install' => true,
        'package_name' => 'redis-server'
      },
      'rabbitmq' => {
        'enabled_plugins' => ['rabbitmq_management'],
        'version' => '3.5.6',
        'use_distro_version' => false
      }
    }

    chef.add_recipe('java')
    chef.add_recipe('ruby_build')
    chef.add_recipe('ruby_rbenv::system')
    chef.add_recipe('redisio::default')
    chef.add_recipe('redisio::enable')
    chef.add_recipe('rabbitmq')
    chef.add_recipe('rabbitmq::plugin_management')
    chef.add_recipe('meetings_recorder::chrome_pt')
    chef.add_recipe('meetings_recorder::chromedriver_pt')
    chef.add_recipe('meetings_recorder::default')
    chef.add_recipe('meetings_recorder::dependencies')
    chef.add_recipe('meetings_recorder::ffmpeg_pt')
  end
end
