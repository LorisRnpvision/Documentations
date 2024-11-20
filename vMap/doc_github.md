## Sommaire
- [GitHub](#github)
  - [Créer un repositeries](#créer-un-repositeries)
  - [Les commandes pour modifier le repo en local](#les-commandes-pour-modifier-le-repo-en-local)
  - [Créer un token](#créer-un-token)


# GitHub 
## Créer un repositeries
* -> Photo de profil
* -> Your repositeries
* -> New
* -> Cochez la case readme
* -> Créez


## Les commandes pour modifier le repo en local
* **Pour récupérer le repo :**
  * `git clone <URL>`
* **Pour récupérer les modifications :**
  * `git pull` (dans le dossier du projet)
* **Ajoutez des modifications :**
  * `git add *`
  * `git commit -m "Message sur les modifs effectuées"`
  * `git push`
  * (Se connecter avec ton utilisateur et [token](#3-créer-un-token-))

## Créer un token
* -> Photo de profil
* -> Settings
* -> Developer settings
* -> Personal access tokens
* -> Tokens (classic)
* -> Generate new token
* -> Note (nom du token)
* -> Expiration (comme vous le voulez, no expiration implique de bien le sécuriser)
* -> Cochez :
    * repo
    * admin:org
    * project 
  
(si problème de droit un jour, regarder le token s'il accepte ce que vous voulez faire)