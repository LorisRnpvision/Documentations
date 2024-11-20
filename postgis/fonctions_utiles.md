<style>
    titre {
        font-weight: bold;
        font-size: 36px;
        color: #FFF;  
        text-align: center;    
        margin-bottom: 20px;
    }

</style>

- [Quelques fonction postGis utile](#quelques-fonction-postgis-utile)
  - [1. ST\_GeometryFromText](#1-st_geometryfromtext)
    - [Appels](#appels)
    - [Retour](#retour)
    - [Lien doc](#lien-doc)
  - [2. ST\_IsValid](#2-st_isvalid)
    - [Appels](#appels-1)
    - [Retour](#retour-1)
    - [Liens doc](#liens-doc)
  - [3. ST\_Point](#3-st_point)
    - [Appels](#appels-2)
    - [Retour](#retour-2)
    - [Info complémentaire](#info-complémentaire)
    - [Liens doc](#liens-doc-1)
  - [4. ST\_IsValid](#4-st_isvalid)
    - [Appels](#appels-3)
    - [Retour](#retour-3)
    - [Liens doc](#liens-doc-2)
  - [5. ST\_AsText](#5-st_astext)
    - [Appels](#appels-4)
    - [Retour](#retour-4)
    - [Liens doc](#liens-doc-3)
  - [6. ST\_Intersects](#6-st_intersects)
    - [Appels](#appels-5)
    - [Retour](#retour-5)
    - [Liens doc](#liens-doc-4)
  - [7. ST\_Transform](#7-st_transform)
    - [Appels](#appels-6)
    - [Retour](#retour-6)
    - [Liens doc](#liens-doc-5)


# Quelques fonction postGis utile

## <titre>1. ST_GeometryFromText</titre>
### Appels
 * geometry ST_GeometryFromText(text WKT);
 * geometry ST_GeometryFromText(text WKT, integer srid);

### Retour
 * Retourn une geometry du type de ce qu'on lui passe (Point, Polygon, Geometry, etc...)

### Lien doc
* https://postgis.net/docs/manual-3.5/fr/ST_GeometryFromText.html


## <titre>2. ST_IsValid</titre>
### Appels
 * boolean ST_IsValid(geometry g);
 * boolean ST_IsValid(geometry g, integer flags);

### Retour
 * Retourne un booléen si la geometry passé est valid ou non (Coordonnées invalides, Polygon non fermé, avec troue, croisement, etc...)

### Liens doc
* https://postgis.net/docs/manual-3.5/fr/ST_IsValid.html


## <titre>3. ST_Point</titre>
### Appels
 * geometry ST_Point(float x, float y);
 * geometry ST_Point(float x, float y, integer srid=unknown);

### Retour
 * Retourne une geometry de type Point avec les coordonnées spécifiés.

### Info complémentaire
* Il existe aussi des fonctions pour les Polygone, etc... Du même type que celle-ci.

### Liens doc
* https://postgis.net/docs/manual-3.5/fr/ST_Point.html

## <titre>4. ST_IsValid</titre>
### Appels
 * integer ST_SRID(geometry g1);

### Retour
 * Retourne le SRID de la geometry passé en paramètre.

### Liens doc
* https://postgis.net/docs/manual-3.5/fr/ST_SRID.html


## <titre>5. ST_AsText</titre>
### Appels
* text ST_AsText(geometry g1);
* text ST_AsText(geometry g1, integer maxdecimaldigits = 15);
* text ST_AsText(geography g1);
* text ST_AsText(geography g1, integer maxdecimaldigits = 15);

### Retour
 * Retourne la représentation WKT de la géometry (souvent utilisé pour stocké en BDD).

### Liens doc
* https://postgis.net/docs/manual-3.5/fr/ST_AsText.html


## <titre>6. ST_Intersects</titre>
### Appels
* boolean ST_Intersects( raster rastA , integer nbandA , raster rastB , integer nbandB );
* boolean ST_Intersects( raster rastA , raster rastB );
* boolean ST_Intersects( raster rast , integer nband , geometry geommin );
* boolean ST_Intersects( raster rast , geometry geommin , integer nband=NULL );
* boolean ST_Intersects( geometry geommin , raster rast , integer nband=NULL );

### Retour
 * Retourne un booléen si les deux géometry s'intersecte.

### Liens doc
* https://postgis.net/docs/manual-3.5/fr/ST_AsText.html


## <titre>7. ST_Transform</titre>
### Appels
* geometry ST_Transform(geometry g1, integer srid);
* geometry ST_Transform(geometry geom, text to_proj);
* geometry ST_Transform(geometry geom, text from_proj, text to_proj);
* geometry ST_Transform(geometry geom, text from_proj, integer to_srid);

### Retour
 * Retourne la géometry dans un autre SRID.

### Liens doc
* https://postgis.net/docs/manual-3.5/fr/ST_Transform.html