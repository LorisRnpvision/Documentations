## Sommaire
- [Apache2](#apache2)
  - [Installation](#installation)
  - [Ajouter une clef SSL a un site local](#ajouter-une-clef-ssl-a-un-site-local)
  - [Quelques commandes utile :](#quelques-commandes-utile-)

# Apache2

## Installation
- `sudo apt install apache`
- `sudo apt install openssl` (Pour les certificats SSL)

## Ajouter une clef SSL a un site local
```bash
echo "[v3_req]" > /tmp/openssl.cnf 
echo "basicConstraints=CA:FALSE" >> /tmp/openssl.cnf

rm /etc/ssl/private/selfsigned.key
rm /etc/ssl/certs/selfsigned.crt

sudo openssl req -x509 -nodes -days 365 -newkey rsa:2048 \
                 -keyout /etc/ssl/private/selfsigned.key \
                 -out /etc/ssl/certs/selfsigned.crt \
                 -subj "/C=FR/ST=France/L=Ploufragan/O=Organization/CN=localhost" \
                 -extensions v3_req -config /tmp/openssl.cnf

rm /tmp/openssl.cnf                

echo "
<VirtualHost *:80>
    ServerName localhost
    DocumentRoot /var/www/vmap

    # Redirection vers HTTPS
    Redirect permanent / https://localhost/vmap
</VirtualHost>
<VirtualHost *:443>
    ServerName localhost
    DocumentRoot /var/www/vmap

    SSLEngine on
    SSLCertificateFile /etc/ssl/certs/selfsigned.crt
    SSLCertificateKeyFile /etc/ssl/private/selfsigned.key

    <Directory /var/www/vmap>
        AllowOverride All
        Require all granted
    </Directory>
</VirtualHost>
" >> /etc/apache2/sites-available/vm_app_vmap.conf

sudo chown -R www-data:www-data /var/www/vmap
sudo chmod -R 755 /var/www/vmap

sudo chmod 644 /etc/ssl/certs/selfsigned.crt
sudo chmod 600 /etc/ssl/private/selfsigned.key

sudo a2ensite vm_app_vmap.conf
sudo a2enmod ssl
sudo systemctl restart avahi-daemon
sudo systemctl restart apache2
sudo apache2ctl configtest
```

## Quelques commandes utile :
* `sudo systemctl start apache2`
* `sudo systemctl restart apache2`
* `sudo systemctl status apache2`
* `apache2 -v`
* Les logs d'apache :
  * `sudo tail -f /var/log/apache2/error.log`
  * `sudo tail -f /var/log/apache2/access.log`