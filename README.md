# dokshinda_infra
dokshinda Infra repository
DZ-3 Первая часть задания:



    Использование bastion для доступа к хосту someinternalhost:

ssh -J appuser@158.160.110.74 appuser@10.128.0.21

    Использовать команду: ssh -o ProxyCommand="ssh -W %h:%p appuser@158.160.110.74 appuser@10.128.0.21


    Отредактировать  ~/.ssh/config

Host someinternalhost

HostName 10.128.0.21

User appuser

ProxyCommand ssh -W %h:%p appuser@158.160.110.74

Команда подключения в данном случае будет выглядеть так:

ssh someinternalhost

______________________________________
vpn настроен согласно инстр.

cat <<EOF> setupvpn.sh

#!/bin/bash

echo "deb http://repo.pritunl.com/stable/apt focal main" | sudo tee /etc/apt/sources.list.d/pritunl.list

apt-key adv --keyserver hkp://keyserver.ubuntu.com --recv 7568D9BB55FF9E5287D586017AE645C0CF8E292A

curl https://raw.githubusercontent.com/pritunl/pgp/master/pritunl_repo_pub.asc | sudo apt-key add -

echo "deb https://repo.mongodb.org/apt/ubuntu focal/mongodb-org/7.0 multiverse" | tee /etc/apt/sources.list.d/mongodb-org-7.0.list

wget -qO - https://www.mongodb.org/static/pgp/server-7.0.asc | sudo apt-key add -

apt update

apt -y install wireguard wireguard-tools

ufw disable

apt -y install mongodb-org

apt -y install pritunl

systemctl enable mongod pritunl

systemctl start mongod pritunl

EOF

sudo bash setupvpn.sh

https://158.160.110.74/setup
