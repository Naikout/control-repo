# control-repo
Puppet course from linkedin learning

# How to run

## Setting up vagrant and puppetmaster

### Vagrant
Get a Vagrantfile for example CentOS 7 (from for example HashiCorp)

Use `vagrant up` command to start vagrant.

Then `vagrant ssh` to connect.

After that, you need to be root so use `sudo su -`.

### Puppet master server

Download puppet using `rpm -Uvh https://yum.puppet.com/puppet6-release-el-7.noarch.rpm`.

Install some required packages using `yum install -y puppetserver vim git`.

Tune `/etc/sysconfig/puppetserver` if you want

Then start the server using `start puppetserver` and check status using `status puppetserver`

### Start puppetserver as a service
Set it to start as a service with vagrant in `etc/puppetlabs/puppet/puppet.conf`
add
`
[agent]
server = master.puppet.vm
`

### Add Ruby and Gem to path from puppetserver bins

`vim ~/.bash_profile`

Add line `PATH=$PATH:/opt/puppetlabs/puppet/bin`

Refresh bash session `exec bash` and `source .bash_profile`

#### Install r10k with gem

r10k allows the use of git with the puppetserver, install it with `gem install r10k`

Then test it with `puppet agent -t`

### SCM

`mkdir /etc/puppetlabs/r10k`

`vim /etc/puppetlabs/r10k/r10k.yaml`

```
---
:cachedir: '/var/cache/r10k'

:sources:
        :my-org:
               remote: 'https://github.com/Naikout/control-repo.git'
               basedir: '/etc/puppetlabs/code/environments'
```
Deploy environment with `r10k deploy environment -p`

Check `/etc/puppetlabs/code/environments` for repo

### Run development sandbox

`r10k deploy environment -p`

`docker kill minetest.puppet.vm`

`docker exec -it minetest.puppet.vm puppet agent -t`
