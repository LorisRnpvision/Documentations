# Installation vMap
## 1. Prérequis :
- **Apache2** :
    - ```sudo apt install apache```
    - ```sudo apt install openssl``` (Pour les certificats SSL)
- **Postgresql** :
    - ```sudo apt-get install postgresql postgresql-contrib```
    - ```sudo nano /etc/postgres/<version>/main/postgresql.conf```
    - Décommenter et mettez ```*``` dans ```#listen_adresses="localhost"```
    - Remplacer ```local all postgres Peer``` par ```local all postgres trust```
    - ```sudo systemctl restart postgres```
    - Dans un autre terminal > Connecter vous dans le terminal avec ```psql -U postgres```
    - Changez de mot de passe avec :
        ```ALTER USER postgres WITH PASSWORD 'new_password';```
    - Mettez ```local all postgres scram-sha-256``` dans ```postgres.conf```
    - Rajoutez ```# IPv4 local connections host: vmap all 127.0.0.1/32 md5```
    - Et ```# IPv6 local connections: host vmap all ::1/128 md5```
    - Décommenter ou mettez ```password_encryption = scram-sha-256```
    - ```sudo systemctl restart postgres```
- **PostGis** :
    - ```sudo apt-get install postgresql-<latestVersion>-postgis-<latestVersion>```


## 2. Installation :
 - Télécharger vMap sur [vStore](https://vstore.veremes.net/vstore/login).
 - Dans le ```dependencies.json``` changer :
    - ```"HOSTNAME": "127.0.0.1"```
    - ```POSTGRES_DB": "vmap"```
    - ```"POSTGRES_HOST": "127.0.0.1"```
    - ```"POSTGRES_PASSWORD": "<Mot de passe postgres>"```
    - ```"POSTGRES_PORT": 5432```
    - ```"POSTGRES_USER": "postgres"```
 - Créer un script bash et copier ceci :
 ```
#!bin/bash

sudo rm -rf /var/www/vmap
sudo ./installer

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
- Lancez ce script dans le même dossier où est l'installé de base de vMap. (Ce script installe vMap, crée un certificat SSL valide pour le local et l'active).
- ```sudo systemctl restart postgresql```

\
**_Les identifiants de base sont : admin, admin_**
\
(à changer dans ```dependencies.json``` tout en bas, puis relancer le script d'installation)

# Testez dans [https://localhost/vmap](https://localhost/vmap) ! 
Si problème, supprimer le schéma vmap dans pgAdmin ainsi que tous les rôles liés à vMap (vmap..., vitis..., u_vitis et aucun autre !). 
<br>
Relancer le script bash ensuite. 
