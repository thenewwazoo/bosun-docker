# -*- mode: ruby -*-
# vi: set ft=ruby :

VAGRANTFILE_API_VERSION = "2"

# From https://docs.docker.com/engine/installation/ubuntulinux/
$script = <<EOF
echo "Installing docker"
apt-key adv --keyserver hkp://p80.pool.sks-keyservers.net:80 --recv-keys 58118E89F3A912897C070ADBF76221572C52609D >> /dev/null
echo "deb https://apt.dockerproject.org/repo ubuntu-trusty main" > /etc/apt/sources.list.d/docker.list
apt-get update -qq
DEBIAN_FRONTEND=noninteractive apt-get install -qq -y --force-yes docker-engine
usermod -aG docker ubuntu >> /dev/null 2&>1 || usermod -aG docker vagrant >> /dev/null 2>&1
echo "Getting docker-compose"
curl -sL https://github.com/docker/compose/releases/download/1.5.2/docker-compose-`uname -s`-`uname -m` > /usr/local/bin/docker-compose
chmod 0755 /usr/local/bin/docker-compose
cd /vagrant/
echo "Starting containers..."
service docker restart
EOF

Vagrant.configure(VAGRANTFILE_API_VERSION) do |config|

  config.vm.provider :aws do |aws, override|
    config.vm.box = "dummy"
    config.vm.box_url = "https://github.com/mitchellh/vagrant-aws/raw/master/dummy.box"

    aws.access_key_id = ENV['AWS_ACCESS_KEY_ID']
    aws.secret_access_key = ENV['AWS_SECRET_ACCESS_KEY']

    aws.ami = "ami-22b9a343"
    aws.instance_type = "t2.micro"
    aws.security_groups = ["default",]
    aws.region = "us-west-2"

    aws.keypair_name = "aws"
    override.ssh.username = "ubuntu"
    override.ssh.private_key_path = ENV['HOME']+"/.ssh/aws"

    aws.tags = {
      'Name' => 'bosun-docker'
    }

  end

  config.vm.provider "virtualbox" do |vb|
    vb.name = "bosun-docker"
    config.vm.box = "ubuntu/trusty64"
  end

  config.vm.provision "shell", inline: $script

end
