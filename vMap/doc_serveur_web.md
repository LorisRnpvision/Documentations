# Apache2

# Installation :
- `sudo apt install apache`
- `sudo apt install openssl` (Pour les certificats SSL)

## Quelques commandes utile :
* `sudo systemctl start apache2`
* `sudo systemctl restart apache2`
* `sudo systemctl status apache2`
* `apache2 -v`
* Les logs d'apache :
  * `sudo tail -f /var/log/apache2/error.log`
  * `sudo tail -f /var/log/apache2/access.log`