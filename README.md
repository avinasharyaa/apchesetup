# apchesetup

first step aws ka host zone ma sa 4 name server ko aad kero hostiger ke name server ke saath 

2nd step appacha ko install kero 
sudo apt update
sudo apt install apache2 certbot python3-certbot-apache -y

sudo systemctl enable apache2
sudo systemctl start apache2

check status 
sudo systemctl status apache2

add ssl 
sudo certbot --apache -d mylawyersai.com -d www.mylawyersai.com
