#Установка OpenConnect server на CA-RTR
apt update
apt install ocserv
systemctl status ocserv
ufw allow 80,443/tcp

#Установка Certbot на CA-RTR
apt install certbot
certbot certonly --standalone --preferred-challenges http --agree-tos 
