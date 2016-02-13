# Vagrantfile API/syntax version. Don't touch unless you know what you're doing!
VAGRANTFILE_API_VERSION = "2"

Vagrant.configure(VAGRANTFILE_API_VERSION) do |config|
  config.vm.box = "phusion-open-ubuntu-14.04-amd64"
  config.vm.box_url = "https://oss-binaries.phusionpassenger.com/vagrant/boxes/latest/ubuntu-14.04-amd64-vbox.box"

  config.vm.provider "virtualbox" do |v|
    v.customize ["modifyvm", :id, "--memory", "2048"]
  end

  if Dir.glob("#{File.dirname(__FILE__)}/.vagrant/machines/default/*/id").empty?
    # Install Docker
    pkg_cmd = "wget -q -O - https://get.docker.io/gpg | apt-key add -;" \
      "echo deb http://get.docker.io/ubuntu docker main > /etc/apt/sources.list.d/docker.list;" \
      "apt-get update -qq; apt-get install -q -y --force-yes lxc-docker; "
    # Add vagrant user to the docker group
    pkg_cmd << "usermod -a -G docker vagrant; "
    # Prepare dependencies
    pkg_cmd << "apt-get install -q -y --force-yes redis-server rabbitmq-server mysql-client git python-pip python-virtualenv libssl-dev;"
    # Setup rabbitmq
    pkg_cmd << "rabbitmqctl add_user test test; rabbitmqctl add_vhost test; rabbitmqctl set_permissions -p test test .\\* .\\* .\\*;"
    pkg_cmd << "virtualenv occo;source occo/bin/activate;pip install --upgrade pip;"
    pkg_cmd << "pip install --find-links http://pip.lpds.sztaki.hu/packages --no-index --trusted-host pip.lpds.sztaki.hu OCCO-API;"
    pkg_cmd << "echo 'source occo/bin/activate' >> .bashrc"
    config.vm.provision :shell, :inline => pkg_cmd
  end
end

