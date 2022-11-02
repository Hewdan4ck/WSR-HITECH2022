#Установка и настройка VPN WireGuard
apt install wireguard -y
wg genkey | tee /etc/wireguard/private.key | wg pubkey > /etc/wireguard/public.key

vim /etc/wireguard/private.key
//Копируем ключ, например:
2Hl2UlyD/xFrDyzIBEkfPa27yKllp0O+7e9023u8sHk=

//Создаем конфигурационный файл для сервера:
vim /etc/wireguard/server.conf
[Interface]
PrivateKey = 2Hl2UlyD/xFrDyzIBEkfPa27yKllp0O+7e9023u8sHk=
Address = 10.77.77.1/30
ListenPort = 51820
SaveConfig = true

ufw allow 51820/udp
systemctl enable wg-quick@server --now;
ss -tunlp | grep :51820;
scp -r /etc/wireguard/ root@BR-RTR.wsr:/etc/wireguard;


#Конвейерная установка на CA-RTR
apt install wireguard -y;
wg genkey | tee /etc/wireguard/private.key | wg pubkey > /etc/wireguard/public.key;
PrKey=`cat /etc/wireguard/private.key`;
echo '
[Interface]
PrivateKey = '$PrKey'
Address = 10.77.77.1/30
ListenPort = 51820
SaveConfig = false' > /etc/wireguard/server.conf;
scp -r -P 177 /etc/wireguard/ root@BR-RTR.wsr:/etc/wireguard;

#Установка и настройка клиента VPN WireGuard
apt install wireguard -y
//Копируем ключи
vim /etc/wireguard/private.key
vim /etc/wireguard/public.key

vim /etc/wireguard/wg0.conf
[Interface]
PrivateKey = CLIENT_PRIVATE_KEY
Address = 10.77.77.2/30

[Peer]
PublicKey = SERVER_PUBLIC_KEY
Endpoint = BR-RTR.wsr:51820
AllowedIPs = 0.0.0.0/0

#Конвейерная установка на BR-RTR
apt install wireguard wireguard-dkms -y;
PubKey=`cat /etc/wireguard/public.key`;
wg genkey | sudo tee /etc/wireguard/private.key | wg pubkey | sudo tee /etc/wireguard/public.key;
PrKey=`cat /etc/wireguard/private.key`;
echo '
[Interface]
PrivateKey = '$PrKey'
Address = 10.77.77.2/30

[Peer]
PublicKey = '$PubKey'
Endpoint = CA-RTR.wsr:51820
AllowedIPs = 0.0.0.0/0' > /etc/wireguard/wg0.conf;
scp -P 177 /etc/wireguard/*.key root@CA-RTR.wsr:/etc/wireguard/;

#CA-RTR
PubKey=`cat /etc/wireguard/public.key`;
echo '

[Peer]
PublicKey = '$PubKey'
AllowedIPs = 0.0.0.0/0, ::/0' >> /etc/wireguard/server.conf
systemctl enable wg-quick@server --now;
ss -tunlp | grep :51820;
