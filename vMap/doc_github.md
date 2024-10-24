# GitHub 
## 1. Créer un repositeries :
* -> Photo de profil
* -> Your repositeries
* -> New
* -> Cochez la case readme
* -> Créez


## 2. Les commandes pour modifier le repo en local :
* **Pour récupérer le repo :**
  * `git clone <URL>`
* **Pour récupérer les modifications :**
  * `git pull` (dans le dossier du projet)
* **Ajoutez des modifications :**
  * `git add *`
  * `git commit -m "Message sur les modifs effectuées"`
  * `git push`
  * (Se connecter avec utilisateur, [token](#3-créer-un-token-))

## 3. Créer un token :
* -> Photo de profil
* -> Settings
* -> Developer settings
* -> Personnal access tokens
* -> Tokens (classic)
* -> Generate new token
* -> Note (nom du token)
* -> Expiration (comme vous le voulez, no expiration implique de bien le sécuriser)
* -> Cochez :
    * repo
    * admin:org
    * project 
  
(si problème de droit un jour, regarder le token s'il accepte ce que vous voulez faire)