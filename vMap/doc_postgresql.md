# Postgresql

## Installation :
* `sudo apt-get install postgresql postgresql-contrib`

## Configuration pour vMap :
* `sudo nano /etc/postgres/<version>/main/postgresql.conf`
* Décommenter et mettez `*` dans `#listen_adresses="localhost"`
* Remplacer `local all postgres Peer` par `local all postgres trust`
* `sudo systemctl restart postgres`
* Dans un autre terminal > Connecter vous dans le terminal avec `psql -U postgres`
* Changez de mot de passe avec :
    `ALTER USER postgres WITH PASSWORD 'new_password';`
* Mettez `local all postgres scram-sha-256` dans `postgres.conf`
* Rajoutez `# IPv4 local connections host: vmap all 127.0.0.1/32 md5`
* Et `# IPv6 local connections: host vmap all ::1/128 md5`
* Décommenter ou mettez `password_encryption = scram-sha-256`
* `sudo systemctl restart postgres`