# NGINX Process to deploy app


## OVH conf 

Sur le site OVH, il faut aller dans `WEB cloud`, `Nom de domaine`, `ifremer.re` puis `Zone DNS`.

Ensuite, il faut `Ajouter une entrée`. On sélectionne le A puis, on renseigne le nom de sous domaine (Ex: seatizenmonitoring.ifremer.re).
On laisse TTL par défaut, et en cible, on met l'adresse IP du serveur, la même qui est en face de `ifremer.re 0 A 51.91.102.139`

## NGINX CONF

On va sur le VPS

On crée un fichier et on l'édite : `sudo nano /etc/nginx/sites-availables/seatizenmonitoring`

Dedans on met :

```
server {
    listen 80;
    server_name seatizenmonitoring.ifremer.re;

    location / {
        proxy_pass http://localhost:3000; # Le port où tourne ton app
        proxy_http_version 1.1;
        proxy_set_header Upgrade $http_upgrade;
        proxy_set_header Connection 'upgrade';
        proxy_set_header Host $host;
        proxy_cache_bypass $http_upgrade;
    }
}
```


On link le fichier `sudo ln -s /etc/nginx/sites-available/seatizenmonitoring /etc/nginx/sites-enabled/`

On redémarre la conf :

`sudo systemctl stop nginx` puis `sudo systemctl start nginx`. S'il y a un message rouge, faire `sudo systemctl status nginx` et corriger le problème.

## SSL CONF

Pour activer le SSL, il faut juste exécuter la commande : `sudo certbot --nginx -d seatizenmonitoring.ifremer.re`


Maintenant le site est en https et les requêtes http sont redirigés vers le https. 
