# Postgresql

## Installation :
* `sudo apt-get install postgresql postgresql-contrib`

## Configuration pour vMap :
* `sudo nano /etc/postgres/<version>/main/postgresql.conf`
* Décommentez et mettez `*` dans `#listen_adresses="localhost"`
* Remplacez `local all postgres Peer` par `local all postgres trust`
* `sudo systemctl restart postgres`
* Dans un autre terminal > Connectez vous dans le terminal avec `psql -U postgres`
* Changez de mot de passe avec :
    `ALTER USER postgres WITH PASSWORD 'new_password';`
* Mettez `local all postgres scram-sha-256` dans `postgres.conf`
* Rajoutez `# IPv4 local connections host: vmap all 127.0.0.1/32 md5`
* Et `# IPv6 local connections: host vmap all ::1/128 md5`
* Décommentez ou mettez `password_encryption = scram-sha-256`
* `sudo systemctl restart postgres`


## Quelques commandes utile :
* `sudo systemctl start postgresql`
* `sudo systemctl restart postgresql`
* `sudo systemctl status postgresql`
* Les logs de postgresql :
  * `sudo tail -f /var/log/postgresql/postgresql-17-main.log`