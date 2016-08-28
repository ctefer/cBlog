---
published: true
layout: post
date: '2016-08-28'
categories: jekyll
---
# RoR Vagrant
As an old school programmer, I'm more comfortable with C/C++, Java, and...that's about it. I work as a contractor for the government so there's not a lot of expansion beyond those two languages as of yet. Sure there are some much older variants that you run into like Ada or Fortran, a bit of database development and such, but the newer, sometimes more useful or sophisticated languages take years to enter the industry. So this was my first (actually second, maybe more on that later) venture into the realm of Ruby on Rails and the awesome power of Vagrant. 
​
## Ruby Intro
​
While this post isn't necessarily about [Ruby On Rails](http://rubyonrails.org/), a small overview would be helpful. Ruby on Rails allows you to build web applications in an object oriented environment. From Wikipedia: 
​
>Ruby is a dynamic, reflective, object-oriented, general-purpose programming language.<

​
Ruby is also an interpreted language which means it doesn't need to be compiled. This is especially useful if you're trying to debug your website; a simple save and refresh is all you need to see the changes. The Rails portion of Ruby is the framework the ruby is deployed on:
​
>Ruby on Rails, or simply Rails, is a web application framework written in Ruby under the MIT License. Rails is a model–view–controller (MVC) framework, providing default structures for a database, a web service, and web pages.<

​
Ruby on Rails (RoR) provides advanced templating functions, an object oriented interpreted environment, and features specific to an HTML environment. With the addition of Gems (a way of packaging Ruby methods and APIs), programmers can utilize other project to improve their Ruby applications. 
​
There are thousands of sites that can help you develop for RoR, but the best place to start with the [Ruby Book](https://www.railstutorial.org/book)
​
## Vagrant
Ever been a part of a programming team and need to get everyone setup on the right development environment before the begin programming? Ever experienced **"But it runs on my machine"**? [Vagrant](https://www.vagrantup.com/) solves this issue by using [VirtualBox](https://www.virtualbox.org/wiki/Downloads) and a setup scripts. The process is fairly simple. Install VirtualBox, install Vagrant, and run `vagrant up` in the environment. Windows developers may especially rejoice as the core Vagrant utilities deploy a Linux VM with your project drive connected directly to the VM at `$/vagrant`. 

#### Side Note
> Vagrant works with many different types of virtual machines (or boxes). While we'll be discussing Ubuntu 64 here, the Vagrant website contains several different platforms and instructions for creating your own vagrant box. 

## My first GitHub Project
More on using GitHub later, but for my first GitHub [project](https://github.com/ctefer/ruby-on-rails-vagrant) I chose to combine these two development tools into a single, deployable, easy to use RoR environment. At the time I had been introduced to both Vagrant and Ruby through an interview process and I wanted to be able to play with the code a bit. There was a simpler way of doing what I wanted through a useful Ruby site called [railsbox](https://railsbox.io/), but I found it complicated for a novice like myself to use. 

To use my project, download the source code and `vagrant up`, and a brand new Ruby environment is deployed that you can immediately start editing within the project folder. To make the process even simpler, I suggest installing `Git`. The `Git Bash` included as a Windows dropdown prepackages a bash environment. 

{% highlight ruby %}
# deploy the environment
$/vagrant up
# connect to the VM
$/vagrant ssh
# cleanup the environment
$/vagrant destroy
{% endhighlight %} 

#### Side Note
> My project is a combination of what I learned during my interview process and a project called [Rails Dev Box](https://github.com/rails/rails-dev-box) which is specific to deploying an environment for working on Ruby.

### Vagrant Setup
Vagrant utilizes script commands and some interpretation to get things running. For this project I segregated the `shell` scripting from the `vagrant setup`. For setup, the Vagrant file contains the ruleset for the deployment. Vagrant can deploy several VMs at once.

{% highlight ruby %}
# -*- mode: ruby -*-
# vi: set ft=ruby :

# '2' provides the vagrant API version
Vagrant.configure('2') do |config|
  # Vagrant VM Box Type
  config.vm.box      = 'ubuntu/wily64'
  # VM Host name
  config.vm.hostname = 'rails-dev-box'

  # Network configurations
  config.vm.network "private_network", ip:"192.168.10.20"

  # Boot shell to run once the VM has been established
  #  in other Vagrant deployments, the "vagrant provision" command is run 
  #  to compile and deploy the application
  config.vm.provision :shell, path: 'bootstrap.sh', keep_color: true

  # VM configuration
  config.vm.provider 'virtualbox' do |v|
    v.memory = 1024
  end
end
{% endhighlight %} 

### Provisioning
Running `vagrant up` will set up the development environment and run the provisioning. `vagrant provision` will only run the provisioning section of the Vagrant file. For this project, provisioning kicks off `bootstrap.sh`, a bash application which sets up Ruby and deploys it as a service. I've posted the whole file at the end of the blog post.

`bootstrap.sh` starts by ensuring the process has not been done before by checking for a lock file. The lock file is generated at the end of the script. Then:

* Generate a swap space for increased performance
* Install required Ruby and Rails applications
  - Optional packages are commented out
* Initialize a service and start

## Conclusion
This was a quick project just to get myself oriented with the Git development environment and along the way I learned a bit about Vagrant and RoR. 

#### `bootstrap.sh`
{% highlight C++ %}
# The output of all these installation steps is noisy. With this utility
# the progress report is nice and concise.
function install {
    echo installing $1
    shift
    apt-get -y install "$@" >/dev/null 2>&1
}

if [ ! -f /var/lock/provision.lock ]; then
  echo adding swap file
  fallocate -l 2G /swapfile
  chmod 600 /swapfile
  mkswap /swapfile
  swapon /swapfile
  echo '/swapfile none swap defaults 0 0' >> /etc/fstab

  echo updating package information
  apt-add-repository -y ppa:brightbox/ruby-ng >/dev/null 2>&1
  apt-get -y update >/dev/null 2>&1

  install Ruby ruby2.3 ruby2.3-dev
  update-alternatives --set ruby /usr/bin/ruby2.3 >/dev/null 2>&1
  update-alternatives --set gem /usr/bin/gem2.3 >/dev/null 2>&1

  echo installing Bundler


  #required packages
  gem install bundler -N >/dev/null 2>&1
  install SQLite sqlite3 libsqlite3-dev
  install Rails rails
  install 'Nokogiri dependencies' libxml2 libxml2-dev libxslt1-dev
  install 'Blade dependencies' libncurses5-dev
  install 'ExecJS runtime' nodejs

  #optional ruby development packages
#   install 'development tools' build-essential
#   install Git git
#   install memcached memcached
#   install Redis redis-server
#   install RabbitMQ rabbitmq-server
#
#   install PostgreSQL postgresql postgresql-contrib libpq-dev
#   sudo -u postgres createuser --superuser vagrant
#   sudo -u postgres createdb -O vagrant activerecord_unittest
#   sudo -u postgres createdb -O vagrant activerecord_unittest2
#
#   debconf-set-selections <<< 'mysql-server mysql-server/root_password password root'
#   debconf-set-selections <<< 'mysql-server mysql-server/root_password_again password root'
#   install MySQL mysql-server libmysqlclient-dev
#   mysql -uroot -proot <<EOL
# CREATE USER 'rails'@'localhost';
# CREATE DATABASE activerecord_unittest  DEFAULT CHARACTER SET utf8 DEFAULT COLLATE utf8_general_ci;
# CREATE DATABASE activerecord_unittest2 DEFAULT CHARACTER SET utf8 DEFAULT COLLATE utf8_general_ci;
# GRANT ALL PRIVILEGES ON activerecord_unittest.* to 'rails'@'localhost';
# GRANT ALL PRIVILEGES ON activerecord_unittest2.* to 'rails'@'localhost';
# GRANT ALL PRIVILEGES ON inexistent_activerecord_unittest.* to 'rails'@'localhost';
# EOL

  # Needed for docs generation.
  update-locale LANG=en_US.UTF-8 LANGUAGE=en_US.UTF-8 LC_ALL=en_US.UTF-8

  echo loading Ruby Service
  mkdir -p /etc/ruby-on-rails-vagrant
  cat > /etc/ruby-on-rails-vagrant/launch.sh <<EOL
#!/bin/sh -

while [ ! -f /vagrant/bin/rails ]
do
    sleep 1
done

cd /vagrant
source ./setup-env.sh
ruby ./bin/rails server --binding=0.0.0.0
EOL

  chmod +x /etc/ruby-on-rails-vagrant/launch.sh

  cat > /etc/systemd/system/ruby.service <<EOL
[Units]
Description="ruby on rails daemon"
After=network.target

[Service]
Type=simple
ExecStart =/bin/sh -c 'exec /etc/ruby-on-rails-vagrant/launch.sh'

[Install]
WantedBy=default.target
EOL

  touch /var/lock/provision.lock

fi

cd /vagrant

echo installing bundle
bundle install

echo starting ruby service
systemctl daemon-reload
systemctl start ruby.service


echo 'all set, rock on!'
{% endhighlight %}





​
