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
SaveConfig = false

systemctl enable wg-quick@server --now;
ss -tunlp | grep :51820;
scp 


#Конвейерная установка на CA-RTR
apt install wireguard -y;
wg genkey | tee /etc/wireguard/private.key | wg pubkey > /etc/wireguard/public.key;
PrKey=`cat /etc/wireguard/private.key`;
echo '
[Interface]
PrivateKey = '$PrKey'
Address = 10.77.77.1/30
ListenPort = 51820
SaveConfig = false ' > /etc/wireguard/server.conf;
systemctl enable wg-quick@server --now;
ss -tunlp | grep :51820;
scp -r /etc/wireguard/ root@BR-RTR.wsr:/etc/wireguard;

#Установка и настройка клиента VPN WireGuard
