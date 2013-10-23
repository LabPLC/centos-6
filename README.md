CentOS 6.4
==========

Manual y ejemplo de configuración para un servidor CentOS 6.4

Instalacion
-----------

**Configuracion basica**

    emacs /root/.bashrc /root/.emacs
    cp /etc/sysconfig/network /etc/sysconfig/network.orig
    emacs /etc/sysconfig/network
    cp /etc/sysconfig/clock /etc/sysconfig/clock.orig
    emacs /etc/sysconfig/clock
    cp /usr/share/zoneinfo/Mexico/General /etc/localtime

**Utilerias y editores de texto**

    yum install yum-utils.x86_64 screen.x86_64 emacs-nox.x86_64 vim-minimal.x86_64

**Depositos EPEL y REMI**

    wget http://dl.fedoraproject.org/pub/epel/6/x86_64/epel-release-6-8.noarch.rpm
    wget http://rpms.famillecollet.com/enterprise/remi-release-6.rpm
    rpm -Uvh remi-release-6*.rpm epel-release-6*.rpm
    cp /etc/yum.repos.d/remi.repo /etc/yum.repos.d/remi.repo.orig
    emacs /etc/yum.repos.d/remi.repo

**Actualizar sistema**

    yum update

**Firewall**

    iptables -A INPUT -i lo -j ACCEPT
    iptables -A INPUT -m conntrack --ctstate RELATED,ESTABLISHED -j ACCEPT
    iptables -A INPUT -p icmp --icmp-type 8 -m conntrack --ctstate NEW -j ACCEPT
    iptables -A INPUT -p tcp --dport 22 -j ACCEPT
    iptables -A INPUT -p tcp --dport 80 -j ACCEPT
    iptables -A INPUT -p tcp --dport 8000:8999 -j ACCEPT

    iptables -P INPUT DROP
    iptables -P FORWARD DROP
    iptables -P OUTPUT ACCEPT

    /etc/init.d/iptables save

**PostgreSQL y PostGIS**

    wget http://yum.postgresql.org/9.3/redhat/rhel-6-x86_64/pgdg-centos93-9.3-1.noarch.rpm
    rpm -Uvh pgdg-centos93-*.rpm
    yum install postgis2_93.x86_64 postgresql93-server.x86_64 postgresql93.x86_64
    service postgresql-9.3 initdb
    chkconfig postgresql-9.3 on
    /etc/init.d/postgresql-9.3 start

**Nginx y PHP-FPM**

    emacs /etc/yum.repos.d/nginx.repo
    yum install nginx.x86_64 php-fpm.x86_64 php-pgsql.x86_64
    chkconfig nginx on
    /etc/init.d/nginx start
    chkconfig php-fpm on
    /etc/init.d/php-fpm start

**NodeJS y NPM (Node Package Manager)**
    
    yum install npm

**Git**

    yum install git.x86_64

**Sudo**

    cp /etc/sudoers /etc/sudoers.orig
    visudo

**Sitio codigo.labplc.mx**

    mkdir /srv/sites/codigo.labplc.mx
    mkdir /srv/sites/codigo.labplc.mx/web
    mkdir /srv/sites/codigo.labplc.mx/web/htdocs
    mkdir /srv/sites/codigo.labplc.mx/log
    chown nginx:nginx /srv/sites/codigo.labplc.mx/log
    mv /etc/nginx/conf.d/default.conf /etc/nginx/conf.d/default.conf.orig
    emacs /etc/nginx/conf.d/default.conf
    echo 'codigo.labplc.mx' > /srv/sites/codigo.labplc.mx/web/htdocs/index.html
    echo '<?php phpinfo(); ?>' > /srv/sites/codigo.labplc.mx/web/htdocs/phpinfo.php
    /etc/init.d/nginx restart

Mantenimiento
-------------

**Agregar usuario**

    useradd -u 100 -g 100 -G wheel -s /bin/bash -m manuel
    passwd manuel

**Crear rol de usuario en PostgreSQL**

    su - postgres -c 'createuser -d manuel'

**Crear carpeta publica de usuario**

    chmod 711 /home/manuel
    mkdir /home/manuel/public_html

**Agregar host virtual**

    mkdir /srv/sites/api.codigo.labplc.mx
    mkdir /srv/sites/api.codigo.labplc.mx/web
    mkdir /srv/sites/api.codigo.labplc.mx/web/htdocs
    mkdir /srv/sites/api.codigo.labplc.mx/log
    chown nginx:nginx /srv/sites/api.codigo.labplc.mx/log
    emacs /etc/nginx/conf.d/api.conf
