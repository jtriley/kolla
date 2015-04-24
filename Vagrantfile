# -*- mode: ruby -*-
# vi: set ft=ruby :

$script = <<SCRIPT
yum install -y epel-release
yum install -y https://rdo.fedorapeople.org/rdo-release.rpm
yum install -y python-devel device-mapper-event-libs docker openstack-{keystone,glance,nova}
mv /usr/bin/docker{,.bak}
curl https://test.docker.com/builds/Linux/x86_64/docker-1.6.0 -o /usr/bin/docker
chmod +x /usr/bin/docker
service docker start
easy_install pip
pip install requests==2.5.3
git clone http://github.com/docker/compose /usr/share/docker-compose
cd /usr/share/docker-compose
pip install -e .
git clone git://github.com/stackforge/kolla.git /usr/share/kolla
echo 'export PATH="/usr/share/kolla/tools:$PATH"' > /etc/profile.d/kolla.sh
cd /usr/share/kolla
./tools/genenv
./tools/kolla pull
./tools/kolla start
./tools/init-runonce
SCRIPT

VAGRANTFILE_API_VERSION = "2"

Vagrant.configure(VAGRANTFILE_API_VERSION) do |config|
  config.vm.box = "jeffmccune/centos7"
  #config.vm.box = 'relativkreativ/centos-7-minimal'
  #config.vm.customize [“modifyvm”, :id, “–memory”, “4096”]
  config.vm.provision "shell" do |s|
    s.privileged = true
    s.inline = $script
  end
end
