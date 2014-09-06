# -*- mode: ruby -*-
# vi: set ft=ruby :

# Vagrantfile API/syntax version. Don't touch unless you know what you're doing!
VAGRANTFILE_API_VERSION = "2"

$welcome_script = <<EOS
  ip=$(ip addr show eth0 | grep eth0 | grep inet | cut -d/ -f1| awk '{print $2}')
  echo "You can now access your instance at http://$ip/"
EOS

Vagrant.configure(VAGRANTFILE_API_VERSION) do |config|
  config.vm.box = "baremettle/debian-7.5"

  config.vm.synced_folder ".", "/vagrant", type: "rsync",
    rsync__auto: true

  config.vm.provision "shell",
    inline: "apt-get -yqqq install python-django python-mongoengine python-pip mongodb-server"

  config.vm.provision "shell",
    inline: "pip -q install djangorestframework django-rest-framework-mongoengine markdown django-filter"

  config.vm.provision "shell",
    inline: "/etc/init.d/mongodb stop && mongod --quiet --rest --jsonp --dbpath=/var/lib/mongodb --fork --logpath=/var/log/mongodb/mongodb.log"

  config.vm.provision "shell",
    inline: "mkdir -p /etc/pimometer && cp /vagrant/conf/pimometer.conf /etc/pimometer/pimometer.conf && chmod a+r /etc/pimometer/pimometer.conf"

  config.vm.provision "shell",
    inline: "/vagrant/bin/mongo_config.py -e 'test_event' -p 5"

  config.vm.provision "shell",
    inline: "/vagrant/bin/daemon.py &"

#not bothering with django for now. just gonna use mongodb rest api
#  config.vm.provision "shell",
#    inline: "/usr/bin/python /vagrant/pimometer/manage.py runserver 0.0.0.0:80 &"

  config.vm.provision "shell", inline: $welcome_script

end
