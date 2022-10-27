#Включение IP маршрутизации на CA-RTR и BR-RTR
vim /etc/sysctl.conf
net.ipv4.ip_forward = 1

sysctl -p

#Установка и настройка OpenConnect server на CA-RTR
apt install debian-archive-keyring dirmngr
vim /etc/apt/sources.list
deb http://deb.debian.org/debian/ stretch main

apt update
apt install ocserv
systemctl status ocserv
sudo ufw allow 654/tcp
sudo ufw allow 654/udp
vim /etc/ocserv/ocserv.conf
auth = "plain[passwd=/etc/ocserv/ocpasswd]"
tcp-port = 654
udp-port = 654
server-cert = /etc/ssl/certs/ssl-cert-snakeoil.pem
server-key = /etc/ssl/private/ssl-cert-snakeoil.key
keepalive = 30
try-mtu-discovery = true
ipv4-network = 10.66.66.0
ipv4-netmask = 255.255.255.248
tunnel-all-dns = true
dns = 8.8.8.8
dns = 8.8.4.4 

#Создание VPN аккаунтов
ocpasswd -c /etc/ocserv/ocpasswd username

#NAT
iptables -t nat -A POSTROUTING -s 192.168.1.0/24 -o eth0 -j MASQUERADE
iptables -t nat -A POSTROUTING -s 192.168.2.0/24 -o eth0 -j MASQUERADE
iptables -t nat -A POSTROUTING -s 10.66.66.0/24 -o eth0 -j MASQUERADE
