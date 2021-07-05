# Desenvolvido pela Consultoria Técnica Gerencianet

time_zone="America/Sao_Paulo"

# configuração Vagrant no provider
Vagrant.configure("2") do |config|
  config.vm.box = "ubuntu/focal64"

  config.ssh.forward_agent = true
  config.vm.network "forwarded_port", guest: 80, host: 8080
  config.vm.synced_folder ".", "/vagrant", type: "virtualbox", owner: "www-data", group: "www-data"

  config.vm.provider "virtualbox" do |vb|
    vb.name = "gn-vagrant-php_ubuntu2004"
    vb.memory = 1024
    vb.customize ["guestproperty", "set", :id, "/VirtualBox/GuestAdd/VBoxService/--timesync-set-threshold", 10000]
    vb.customize ["modifyvm", :id, "--natdnshostresolver1", "on"]
    vb.customize ["modifyvm", :id, "--natdnsproxy1", "on"]
  end

  config.vm.provision "shell" do |s|
    s.args = [time_zone]
    s.inline = <<-SHELL
# configuração

TIME_ZONE=$1

# desativa consultas para interações manuais do usuário
export DEBIAN_FRONTEND=noninteractive

# atualiza fuso horário
timedatectl set-timezone "$TIME_ZONE"

# adiciona repositório PHP
add-apt-repository ppa:ondrej/php -y

# atualiza pacotes
apt-get update -q -y

# instala Nano and Git
apt-get install -q -y nano git

# instala Apache e PHP
apt install php8.0 libapache2-mod-php8.0 -q -y

# instala extensões PHP
apt-get install -q -y php8.0-curl php8.0-cli php8.0-common php8.0-mbstring php8.0-xml

a2enmod rewrite headers
systemctl restart apache2

# defini a pasta www_gn como pasta raiz do Apache
dir='/vagrant/www_gn'

if [ ! -d "$dir" ]; then
  sudo mkdir "$dir"
fi

if [ ! -L /var/www/html ]; then
  rm -rf /var/www/html
  ln -fs "$dir" /var/www/html
fi

cd "$dir"

# Configuração do VirtualHost
file='/etc/apache2/sites-available/dev.conf'

if [ ! -f "$file" ]; then
  SITE_CONF=$(cat <<EOF
  <Directory /vagrant/www_gn>
    AllowOverride All
    Options +Indexes -MultiViews +FollowSymLinks
    AddDefaultCharset utf-8
    SetEnv ENVIRONMENT "development"
    php_flag display_errors On
    EnableSendfile Off
  </Directory>
EOF
)
  echo "$SITE_CONF" > "$file"
fi

a2ensite dev
systemctl reload apache2

# instala extensões Zip
apt-get install -q -y zip unzip php-zip

# instala o Composer
EXPECTED_SIGNATURE="$(wget -q -O - https://composer.github.io/installer.sig)"
php -r "copy('https://getcomposer.org/installer', 'composer-setup.php');"
ACTUAL_SIGNATURE="$(php -r "echo hash_file('SHA384', 'composer-setup.php');")"

if [ "$EXPECTED_SIGNATURE" != "$ACTUAL_SIGNATURE" ]; then
  >&2 echo 'ERROR: Invalid installer signature (Composer)'
  rm composer-setup.php
else
  php composer-setup.php --quiet
  rm composer-setup.php

  mv composer.phar /usr/local/bin/composer
  chmod +x /usr/local/bin/composer
fi

# cria um arquivo com phpinfo script
file='phpinfo.php'

if [ ! -f "$file" ]; then
  echo '<?php phpinfo();' > "$file"
fi

SHELL
  end
end
