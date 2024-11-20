# Créer un bouton qui rempli des champs du formulaire

## 1. Créer le bouton dans le formulaire (édition ou saisie).
## 2. Lui mettre en event, le nom d'une fonction que l'on va définir après.
## 3. Dans la définition JS du form créer la fonction dans la fonction déjà présente.
## 4. Voici des fonctions pour remplir la parcelle cadastrale ou plus tout seul :
```js
/**
* constructor_form
* Fonction lancée au chargement du formulaire
* @param { object } scope Formreader
* @param { object } env environment
* @param { object } services
* @returns { undefined } 
*/
var constructor_form = function (scope, env, services) {
    // Fonction dans le insert pour trouver le num de pacrelle et le nom de la commune.
    async function findParcelle() {
        const longitudeLambert = scope.map.interactions.array_[9].sketchCoords_[1];
        const latitudeLambert = scope.map.interactions.array_[9].sketchCoords_[0];
        
        // Convertir les coordonnéés de lambert93 à WGS 83 pour les passer à l'API.
        $.getScript("https://cdnjs.cloudflare.com/ajax/libs/proj4js/2.14.0/proj4.js", async function () {
            // Définition des projections que l'on veut utiliser. 
            proj4.defs([
                [
                    'EPSG:2154',
                    '+proj=lcc +lat_1=49 +lat_2=44 +lat_0=46.5 +lon_0=3 +x_0=700000 +y_0=6600000 +ellps=GRS80 +towgs84=0,0,0,0,0,0,0 +units=m +no_defs'
                ],
                [
                    'WGS84',
                    '+title=WGS 84 (long/lat) +proj=longlat +ellps=WGS84 +datum=WGS84 +units=degrees'
                ]
            ]);

            const lambert93 = proj4('EPSG:2154');
            const wgs84 = proj4('WGS84');

            // Procéder à la reprojection.
            const coordsWGS84 = proj4(lambert93, wgs84, [latitudeLambert, longitudeLambert]);

            // Appeler l'API avec le point ou l'on souhaite recevoir le numéro de pacerelle.
            const url = `https://apicarto.ign.fr/api/cadastre/parcelle?geom={%22type%22:%22Point%22,%22coordinates%22:[${coordsWGS84[0]},${coordsWGS84[1]}]}`;
            let parcelle = null;

            // Tester sa réponse. Si ok, enregistrer les données que l'on veut.
            let resp = await services.vitisRequest.ajaxRequestPromise({
                method: 'GET',
                url: url
            }).then(resp => {
                if (resp.status === 200) {
                    parcelle = resp.data.features[0].properties.numero;
                } else {
                    console.error('Erreur lors de la récupération des informations de la parcelle.');
                }
            }).catch(err => {
                alert("Aucune parcelle trouvée.");
            });

            // Remplir la partir du formulaire en direct.
            services.oFormReader.getFormField('num_pacr').setValue(parcelle);
        });
    }

    // Fonction dans l'update pour trouver le num de pacerelle et le nom de commune.
    async function findParcelleUpdate() {
        const longitudeLambert = scope.clickCoords[1];
        const latitudeLambert = scope.clickCoords[0];

        // Convertir les coordonnéés de lambert93 à WGS 83 pour les passer à l'API.
        $.getScript("https://cdnjs.cloudflare.com/ajax/libs/proj4js/2.14.0/proj4.js", async function () {
            // Définition des projections que l'on veut utiliser. 
            proj4.defs([
                [
                    'EPSG:2154',
                    '+proj=lcc +lat_1=49 +lat_2=44 +lat_0=46.5 +lon_0=3 +x_0=700000 +y_0=6600000 +ellps=GRS80 +towgs84=0,0,0,0,0,0,0 +units=m +no_defs'
                ],
                [
                    'WGS84',
                    '+title=WGS 84 (long/lat) +proj=longlat +ellps=WGS84 +datum=WGS84 +units=degrees'
                ]
            ]);

            const lambert93 = proj4('EPSG:2154');
            const wgs84 = proj4('WGS84');

            // Procéder à la reprojection.
            const coordsWGS84 = proj4(lambert93, wgs84, [latitudeLambert, longitudeLambert]);

            // Appeler l'API avec le point ou l'on souhaite recevoir le numéro de pacerelle.
            const url = `https://apicarto.ign.fr/api/cadastre/parcelle?geom={%22type%22:%22Point%22,%22coordinates%22:[${coordsWGS84[0]},${coordsWGS84[1]}]}`;
            let parcelle = null;

            // Tester sa réponse. Si ok, enregistrer les données que l'on veut.
            try {
                let resp = await services.vitisRequest.ajaxRequestPromise({
                    method: 'GET',
                    url: url
                });
                if (resp.status === 200) {
                    parcelle = resp.data.features[0].properties.numero;
                } else {
                    console.error('Erreur lors de la récupération des informations de la parcelle.');
                }
            } catch (err) {
                alert("Aucune parcelle trouvée.");
            }

            // Remplir la partir du formulaire en direct.
            services.oFormReader.getFormField('num_pacr').setValue(parcelle);
        });
    }

    // Ajouter les fonctions au scope pour que le bouton est accès.
    scope.findParcelle = findParcelle;
    scope.findParcelleUpdate = findParcelleUpdate;
}
```