<style>
    erreur {
        color:red;
        background-color: #3d3c3d;
        border-radius: 5px;

        padding-bottom: 2px;
        padding-top: 2px;
        padding-left: 3px;
        padding-right: 3px;
    }

    sousTitre {
        color: white;
        font-size: 1.25em;
        text-decoration: white underline;
    }
</style>
# Rendre vMap fonctionnel
## 1. Problème de droits :
* <sousTitre> Si les couches de base de s'affiche pas, ou qu'il y a des problèmes, comme : </sousTitre>
    * <erreur>Erreur pendant la requête</erreur>
    * <erreur>Vous n'avez pas accès</erreur>
    * Etc...

* <sousTitre>Ajouter les droits à votre utilisateur : </sousTitre>
    * Aller sur pgAdmin ([Installer pgAdmin](#installer-pgadmin-desktop-))
    * Connecter vous à votre base de donnée localhost
    * Aller dans les `Login/Group Roles`
    * Clique droit sur l'utilisateur `u_vitis`
    * Aller dans `Properties` puis `Membreship`
    * Ajouter tous les droits un par un qui comme par `vmap_` ou `vitis_`
    * Ainsi que `pg_read_all_data` et `pg_write_all_data`
    * Cliquez sur save.


## Rajouter des couches dans vMap :


## Installer pgAdmin desktop :
```bash
curl -fsS https://www.pgadmin.org/static/packages_pgadmin_org.pub | sudo gpg --dearmor -o /usr/share/keyrings/packages-pgadmin-org.gpg

sudo sh -c 'echo "deb [signed-by=/usr/share/keyrings/packages-pgadmin-org.gpg] https://ftp.postgresql.org/pub/pgadmin/pgadmin4/apt/$(lsb_release -cs) pgadmin4 main" > /etc/apt/sources.list.d/pgadmin4.list && apt update'

sudo apt install pgadmin4-desktop
```