# -*- mode: ruby -*-
# vi: set ft=ruby :

VAGRANTFILE_API_VERSION = "2"

# From https://docs.docker.com/engine/installation/ubuntulinux/
$setup_bosun = <<EOF
echo "Installing docker"
apt-key adv --keyserver hkp://p80.pool.sks-keyservers.net:80 --recv-keys 58118E89F3A912897C070ADBF76221572C52609D >> /dev/null
echo "deb https://apt.dockerproject.org/repo ubuntu-trusty main" > /etc/apt/sources.list.d/docker.list
apt-get update -qq
DEBIAN_FRONTEND=noninteractive apt-get install -qq -y --force-yes docker-engine
usermod -aG docker ubuntu || usermod -aG docker vagrant
echo "Getting docker-compose"
curl -sL https://github.com/docker/compose/releases/download/1.5.2/docker-compose-`uname -s`-`uname -m` > /usr/local/bin/docker-compose
chmod 0755 /usr/local/bin/docker-compose
cd /vagrant/
echo "Starting containers..."
service docker restart
EOF

$setup_scollector = <<EOF
echo "Getting scollector"
wget -qq -O /usr/local/bin/scollector https://github.com/bosun-monitor/bosun/releases/download/0.5.0-alpha/scollector-linux-amd64
chmod a+x /usr/local/bin/scollector
EOF

Vagrant.configure(VAGRANTFILE_API_VERSION) do |config|

  config.vm.define "bosun" do |config_bosun|
    config_bosun.vm.provider :aws do |bosun, override|
      config_bosun.vm.box = "dummy"
      config_bosun.vm.box_url = "https://github.com/mitchellh/vagrant-aws/raw/master/dummy.box"

      bosun.access_key_id = ENV['AWS_ACCESS_KEY_ID']
      bosun.secret_access_key = ENV['AWS_SECRET_ACCESS_KEY']

      bosun.ami = "ami-22b9a343"
      bosun.instance_type = "t2.micro"
      bosun.security_groups = ["default",]
      bosun.region = "us-west-2"

      bosun.keypair_name = "aws"
      override.ssh.username = "ubuntu"
      override.ssh.private_key_path = ENV['HOME']+"/.ssh/aws"
      override.vm.provision "shell", inline: $setup_bosun

      bosun.tags = {
        'Name' => 'bosun-docker'
      }
    end
  end

  config.vm.define "scollector" do |config_scollector|
    config_scollector.vm.provider :aws do |scollector, override|
      config_scollector.vm.box = "dummy"
      config_scollector.vm.box_url = "https://github.com/mitchellh/vagrant-aws/raw/master/dummy.box"

      scollector.access_key_id = ENV['AWS_ACCESS_KEY_ID']
      scollector.secret_access_key = ENV['AWS_SECRET_ACCESS_KEY']

      scollector.ami = "ami-22b9a343"
      scollector.instance_type = "t2.micro"
      scollector.security_groups = ["default",]
      scollector.region = "us-west-2"

      scollector.keypair_name = "aws"
      override.ssh.username = "ubuntu"
      override.ssh.private_key_path = ENV['HOME']+"/.ssh/aws"
      override.vm.provision "shell", inline: $setup_scollector

      scollector.tags = {
        'Name' => 'bosun-docker'
      }
    end
  end

end
