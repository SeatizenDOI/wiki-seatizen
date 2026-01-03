# Photogrammetrie avec OpendroneMap

Les paramètres de la camera:

LINEAR, ISO 100-400

Génération des orthos de drone et de planche jusqu'en novembre 2025, liste des options :

Tout les orthos sont faites avec la release d'odm 3.5.3. La 3.5.5 a un problème avec les tag COG.


- fast-orthophoto => Pas de modèle 3D et la reconstruction va plus vite.
- cog => Les fichiers sont généré pour être optimisé pour le cloud. Plus facile pour visualiser dans qgis.
- camera-lens => auto
- feature-quality => ultra
- gps_accuracy => 0.1
- orthophoto_resolution => 0.1
- rolling_shutter => True  On l'utilise car la gogpro ne prends pas tous les pixels d'un coup. https://www.lesnumeriques.com/photo/qu-est-ce-que-le-rolling-shutter-pu102253.html

En novembre 2025

- rolling-shutter-readout 9 => La valeur par défaut d'odm est paramétré pour des photos et pas des videos. La valeur dans les métadonnées des vidéos est 9ms.
- On a essayé de filer en WIDE et de mettre camera-lens en fisheye mais ce n'est pas mieux.


## Pourquoi on a pas de 3D ?

En enlevant l'argument fast-orthophoto, on peut avoir un modèle 3D mais la reconstruction est un peu moins bien et plus longue.


