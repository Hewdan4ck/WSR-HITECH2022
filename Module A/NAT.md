#NAT на CA-RTR
iptables -t nat -A POSTROUTING -s 192.168.1.0/24 -o <Интерфейс с выходом в инет> -j MASQUERADE;
apt install debian-archive-keyring dirmngr -y;
echo ' 
deb http://deb.debian.org/debian/ stretch main' >> /etc/apt/sources.list;
apt install iptables-persistent -y;

#NAT на BR-RTR
iptables -t nat -A POSTROUTING -s 192.168.2.0/24 -o <Интерфейс с выходом в инет> -j MASQUERADE;
apt install debian-archive-keyring dirmngr -y;
echo ' 
deb http://deb.debian.org/debian/ stretch main' >> /etc/apt/sources.list;
apt install iptables-persistent -y;
