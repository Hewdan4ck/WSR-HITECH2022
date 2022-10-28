#NAT на CA-RTR
iptables -t nat -A POSTROUTING -s 192.168.1.0/24 -o <Интерфейс с выходом в инет> -j MASQUERADE;
iptables-save > /etc/iptables.rules;

#NAT на BR-RTR
iptables -t nat -A POSTROUTING -s 192.168.2.0/24 -o <Интерфейс с выходом в инет> -j MASQUERADE;
iptables-save > /etc/iptables.rules;
