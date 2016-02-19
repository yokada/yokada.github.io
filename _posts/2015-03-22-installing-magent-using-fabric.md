---
layout: post
title: Installing magento using fabric
tags: fabric, magento
---

my fabfile.py would be like this:

{% highlight python %}
import os
import os.path
from pprint import pprint
import re
from fabric.api import *
from fabric.contrib import console
from contextlib import contextmanager
import sys
#import logging
#logging.basicConfig(level=logging.DEBUG)

local_ip = '192.168.34.10'
ssh_ip = '127.0.0.1'
ssh_port = '2201'
env.hosts = 'vagrant@%s:%s' % (ssh_ip, ssh_port)
env.warn_only = False
base_path = '/host-os/path/to/ec-dev/'
ec_path = base_path+'dev.magento.local/'
template_path = base_path+'template/'
vagrant_config = base_path+'.vagrant'
vagrantfile_path = base_path+'Vagrantfile'
err_msgs = {
    'vagrantfile_exists': '%s is exists' % vagrantfile_path
}
box_name = 'centos64_vb'
vagrantfile_template = '''\
Vagrant.configure(2) do |config|
  config.vm.box = "%(box_name)s"
  config.vm.network "private_network", ip: "%(local_ip)s"
  config.vm.synced_folder "%(ec_path)s", "/var/www/dev.magento.local"
  config.vm.synced_folder "%(template_path)s", "/home/vagrant/template"
end
'''

def init():
    if os.path.exists(vagrantfile_path):
        if prompt("%s exists. are you sure overwrite it? [y/n]" % vagrantfile_path, default="n") != "y":
            print "init task stopped."
            return
        else:
            vm_halt()
            local('rm -rf %s' % vagrant_config)
            local('rm -rf %s' % vagrantfile_path)

    with lcd(base_path):
        local('vagrant init %s' % box_name)
        with open(vagrantfile_path, 'w') as f:
            vagrantfile = vagrantfile_template % {'box_name':box_name, 'local_ip': local_ip, 'ec_path': ec_path, 'template_path': template_path}
            f.write(vagrantfile)
        local('vagrant up')

    #sudo('yum groupinstall "Development tools"')
    with settings(warn_only=True):
        sudo('yum install -y wget lv tree lsof mlocate')
        sudo('rpm -Uvh http://dl.fedoraproject.org/pub/epel/6/x86_64/epel-release-6-8.noarch.rpm')
        sudo('rpm -Uvh http://pkgs.repoforge.org/rpmforge-release/rpmforge-release-0.5.3-1.el6.rf.x86_64.rpm')
        sudo('rpm -Uvh http://rpms.famillecollet.com/enterprise/remi-release-6.rpm')
        sudo('yum install -y groupinstall "Development Tools"')
        sudo('yum install -y --enablerepo=remi httpd httpd-devel mod_ssl openssl mysql-server mysql-devel')
        sudo('yum install -y --enablerepo=remi-php55 php php-devel php-mbstring php-pdo php-gd')
        sudo('yum update -y --enablerepo=remi --enablerepo=rpmforge')

        # install python
        with cd('/home/vagrant'):
            run('wget https://www.python.org/ftp/python/2.7.8/Python-2.7.8.tgz')
            run('tar zxvf Python-2.7.8.tgz')
            with cd('Python-2.7.8'):
                run('./configure --prefix=/usr/local')
                run('make')
                sudo('make install')
            run('wget https://bootstrap.pypa.io/ez_setup.py')
            sudo('python ez_setup.py')
            sudo('easy_install pip')

        # setup php
        sudo('pecl install xdebug')

        # setup mysql
        run("mysqladmin -u root password 'root'")

        # place config files
        sudo('cp -a /home/vagrant/template/apache/server.crt /etc/httpd/conf/server.crt')
        sudo('cp -a /home/vagrant/template/apache/server.key /etc/httpd/conf/server.key')
        sudo('cp -a /home/vagrant/template/apache/ssl.conf /etc/httpd/conf.d/ssl.conf')
        sudo('cp -a /home/vagrant/template/apache/php.conf /etc/httpd/conf.d/php.conf')
        sudo('cp -a /home/vagrant/template/apache/httpd.conf /etc/httpd/conf/httpd.conf')
        sudo('cp -a /home/vagrant/template/php/xdebug.ini /etc/php.d/xdebug.ini')

        # start httpd -DSSL
        sudo('service httpd startssl')
        sudo('exit')

def tmp():
    local('mkdir %s' % ec_path)

def vm_up():
    with lcd(base_path):
        local('vagrant up')

@task
def vm_halt():
    with lcd(base_path):
        local('vagrant halt')

def rollback():
    print "rollback"

@contextmanager
def rollbackwrap():
    try:
        yield
    except SystemExit:
        rollback()
        abort("Error has occurred while running task!")

@task
def setup():
    with rollbackwrap():
        init()
{% endhighlight %}
