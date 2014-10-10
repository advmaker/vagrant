# -*- mode: ruby -*-
# vi: set ft=ruby :

# bootstrap script partially copied from https://github.com/Lukx/vagrant-lamp
$script = <<SCRIPT
    export VAGRANT_SYNCED_DIR=/var/www # or put $1

    apt-get update

    # apache2 server install'n'config
    apt-get -y install apache2
    echo "ServerName localhost" > /etc/apache2/conf-enabled/servername.conf

    sed -i "s#DocumentRoot /var/www/html#DocumentRoot ${VAGRANT_SYNCED_DIR}#" /etc/apache2/sites-enabled/000-default.conf
    sed -i "s#<Directory /var/www/html>#<Directory ${VAGRANT_SYNCED_DIR}/>#" /etc/apache2/sites-enabled/000-default.conf
    rm -R /var/www/html/

    # php, mysql, nodejs
    apt-get -y install nodejs php5-cli libapache2-mod-php5 php5-mysql php5-curl npm

    ln -s /usr/bin/nodejs /usr/bin/node
    npm install bower -g
    npm install gulp -g


    # composer install
    curl -sS https://getcomposer.org/installer | php
    mv composer.phar /usr/local/bin/composer

    debconf-set-selections <<< 'mysql-server mysql-server/root_password password root'
    debconf-set-selections <<< 'mysql-server mysql-server/root_password_again password root'
    apt-get -y install mysql-server

    apt-get -y install libapache2-mod-auth-mysql php5-mysql

    service apache2 restart
SCRIPT

# Vagrantfile API/syntax version. Don't touch unless you know what you're doing!
VAGRANTFILE_API_VERSION = "2"

Vagrant.configure(VAGRANTFILE_API_VERSION) do |config|

    config.vm.box = "ubuntu/trusty64"
    config.vm.network :forwarded_port, host: 4567, guest: 80

    config.vm.synced_folder "./", "/var/www"

    # provisioner config
    config.vm.provision "shell", inline: $script
end
