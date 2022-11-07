#Установка дебиан репозитория
apt install debian-archive-keyring dirmngr -y;
echo ' 
deb http://deb.debian.org/debian/ stretch main' >> /etc/apt/sources.list;

#Настройка IP маршрутизации
echo net.ipv4.ip_forward=1 >> /etc/sysctl.conf;
sysctl -p;

#Настройка DHCP
apt install fly-admin-dhcp;
vim /etc/default/isc-dhcp-server;
INTERFACEv4="<Интерфейс для DHCP трафика>"
#INTERFACEv6 - комментируем

echo '
ddns-update-style interim;
subnet 10.0.0.0 netmask 255.255.255.0 {
	range 10.0.0.10 10.0.0.254;
	option routers 10.0.0.1;
	option subnet-mask 255.255.255.0;
	option broadcast-address 10.0.0.255;
	option domain-name-servers 10.0.0.1;
	ddns-updates on;
	ddns-domainname "wsr";
	allow client-updates;
	zone wsr {
		primary 10.0.0.1;
	}
} ' > /etc/dhcp/dhcpd.conf;

#NAT
iptables -t nat -A POSTROUTING -s 0.0.0.0/0 -о <Интерфейс с выходом в интернет> -j MASQUERADE
apt install iptables-persistent -y
https://frank.sauerburger.io/2018/05/27/make-your-iptables-rules-persistent-in-centos-7.html

#DNS
apt install bind9 bind9utils dnsutils -y;
ufw allow Bind9;
echo '
options {
	directory "/var/cache/bind";
	forwarders {
		8.8.8.8;
	};
	listen-on { 10.0.0.0/8; };
	allow-query { any; };
	dnssec-validation no;
	recursion yes;
	auth-nxdomain yes;
	listen-on-v6 { any; };
}; ' > /etc/bind/named.conf.options;
echo '
zone "wsr" {
        type master;
        file "/opt/dns/wsr";
        allow-transfer { any; };
        allow-update { any; };
};' >> /etc/bind/named.conf.default-zones;
mkdir /opt/dns/;
chown bind:bind /opt/dns -R;
touch /opt/dns/wsr;
named-checkconf;
systemctl restart bind9;
