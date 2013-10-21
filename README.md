CentOS 6.4
==========

Manual y ejemplo de configuración para un servidor CentOS 6.4

** Utilerias, editores de texto y dotfiles **

    yum install yum-utils screen.x86_64 emacs-nox.x86_64 vim-minimal.x86_64
    emacs /root/.bashrc /root/.emacs

** Depositos EPEL y REMI **

    wget http://dl.fedoraproject.org/pub/epel/6/x86_64/epel-release-6-8.noarch.rpm
    wget http://rpms.famillecollet.com/enterprise/remi-release-6.rpm
    rpm -Uvh remi-release-6*.rpm epel-release-6*.rpm
    cp /etc/yum.repos.d/remi.repo /etc/yum.repos.d/remi.repo.orig
    emacs /etc/yum.repos.d/remi.repo

** Actualizar sistema **

    yum update

** Configuracion basica **

    cp /etc/sysconfig/network /etc/sysconfig/network.orig
    emacs /etc/sysconfig/network
    cp /etc/sysconfig/clock /etc/sysconfig/clock.orig
    emacs /etc/sysconfig/clock
    cp /usr/share/zoneinfo/Mexico/General /etc/localtime

** Firewall **

    iptables -A INPUT -m conntrack --ctstate RELATED,ESTABLISHED -j ACCEPT
    iptables -A INPUT -p icmp --icmp-type 8 -m conntrack --ctstate NEW -j ACCEPT
    iptables -A INPUT -p tcp --dport 22 -j ACCEPT
    iptables -A INPUT -p tcp --dport 80 -j ACCEPT

    iptables -P INPUT DROP
    iptables -P FORWARD DROP
    iptables -P OUTPUT ACCEPT

** Servidor Web **

    emacs /etc/yum.repos.d/nginx.repo
    yum install nginx.x86_64
    chkconfig nginx on
