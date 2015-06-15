Vagrant.configure("2") do |config|

  config.vm.hostname = "lietuva"
  config.vm.box = "svajone/lietuva"
  config.ssh.forward_agent = true

  config.vm.provider 'virtualbox' do |vbox|
    vbox.customize ["modifyvm", :id, "--name", "gediminas"]
    vbox.customize ['modifyvm', :id, '--memory', 4096]
    vbox.customize ['modifyvm', :id, '--ioapic', 'on', '--cpus', 4]
    vbox.customize ["modifyvm", :id, "--natdnshostresolver1", "on"]
  end

  config.vm.network "private_network", ip: "192.68.69.69"

  config.vm.provision :chef_solo do |chef|
    chef.cookbooks_path = ["cookbooks"]
    chef.add_recipe 'apt'
    chef.add_recipe 'python'
    chef.add_recipe 'sqlite'
    chef.add_recipe 'git'
    chef.add_recipe 'ruby_build'
    chef.add_recipe 'rbenv::user'
    chef.add_recipe 'nodejs'
    chef.add_recipe 'postgresql::server'
    chef.add_recipe 'redis'
    chef.json = {
      :git        => {
        :prefix => "/usr/local"
      },
      :rbenv      => {
        :user_installs => [
          {
            :user   => "vagrant",
            :rubies => [
              "2.2.0",
              "2.2.1",
              "2.2.2"
            ],
            :global => "2.2.2"
          }
        ]
      },
      :postgresql => {
        :config   => {
          :listen_addresses => "*",
          :port             => "5432"
        },
        :pg_hba   => [
          {
            :type   => "local",
            :db     => "postgres",
            :user   => "postgres",
            :addr   => nil,
            :method => "trust"
          },
          {
            :type   => "host",
            :db     => "all",
            :user   => "all",
            :addr   => "0.0.0.0/0",
            :method => "md5"
          },
          {
            :type   => "host",
            :db     => "all",
            :user   => "all",
            :addr   => "::1/0",
            :method => "md5"
          }
        ],
        :password => {
          :postgres => "gedimnas"
        }
      },
      :redis      => {
        :bind        => "127.0.0.1",
        :port        => "6379",
        :config_path => "/etc/redis/redis.conf",
        :daemonize   => "yes",
        :timeout     => "300",
        :loglevel    => "notice",
        :password    => true
      }
    }
  end
end
