#Установка и настройка VPN wireguard
apt install wireguard -y
wg genkey | tee /etc/wireguard/private.key | wg pubkey > /etc/wireguard/public.key

vim /etc/wireguard/private.key
//Копируем ключ, например:
2Hl2UlyD/xFrDyzIBEkfPa27yKllp0O+7e9023u8sHk=

//Создаем конфигурационный файл для сервера:
vim /etc/wireguard/server.conf
[Interface]
PrivateKey = 2Hl2UlyD/xFrDyzIBEkfPa27yKllp0O+7e9023u8sHk=
Address = 10.77.77.1/24
ListenPort = 51820
SaveConfig = false
