#Настройка IP маршрутизации
echo net.ipv4.ip_forward=1 >> /etc/sysctl.conf;
sysctl -p;

#Настройка DHCP
apt install fly-admin-dhcp;
vim /etc/default/isc-dhcp-server;
INTERFACEv4="<Интерфейс для DHCP трафика>"
#INTERFACEv6 - комментируем

vim /etc/dhcp/dhcpd.conf
subnet 10.0.0.0 netmask 255.255.255.0 {
	range 10.0.0.10 10.0.0.254
	option routers 10.0.0.1
	option subnet-mask 255.255.255.0
	option broadcast-address 10.0.0.255
	option domain-name-servers 10.0.0.1;
}

#NAT
iptables -t nat -A POSTROUTING -s 0.0.0.0/0 -о <Интерфейс с выходом в интернет> -j MASQUERADE
iptables-save > /etc/iptables.rules

#DNS
apt install bind9 bind9utils dnsutils -y;
ufw allow Bind9;
vim /etc/bind/named.conf.options
forwarders {
	8.8.8.8;
};
listen-on { 10.0.0.0/8; };
allow-query { any; };

named-checkconf;
systemctl restart bind9;
