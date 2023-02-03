Lorsque je tape l'adresse : http://localhost/apprenant1/public/
1/ le message suivant apparait en haut de la page
   Warning: Trying to access array offset on value of type bool in C:\laragon\www\apprenant1\app\Controllers\CoreController.php on line 1
2/ Dans la console de developpeur je vois apparaitre le message d'erreur suivant :
   Failed to load resource: the server responded with a status of 404 (Not Found) pour les ressources  : 
	http://localhost/apprenant1/public/
	http://localhost/apprenant1/public/css/style.css
3/ Le message 404, je devrais etre redirige vers la page de connexion public/signin

Lorsque je clique sur http://localhost/signin
J'obtiens une erreur 404


Les erreurs de type 404 signifie qu'une ressource n'est pas trouvé à l'endroit indiqué.
par exemple ton fichier de vue ici header.tpl.php contient le liens suivant  
    <!-- We can still have our own CSS file -->
    <link rel="stylesheet" href="./css/style.css">

Maintenant tu as un probleme de routage et de redirection
cela vient de ta variable $match qui contient les routes celle-ci est de type booleen et ne peut donc contenir tes routes.
En fait la valeur est FALSE en regardant la documentation de la librairie cela signifie qu'il ne trouve pas la route en question.
La question est pourquoi ?
Parceque tes routes enregistrées sont :
/public /teachers /students/list /teachers/add /students/add /signin /logout /403 /404 /appusers
mais les requetes envoyées sont prefixes par /apprenant1 et suffixe par un / exemple /apprenant1/public/
Il faut donc:
1/ Il faut donc rediriger toutes le requetes sur index.php qui sert de front controller et qui va recuperer les requetes et deleguer le traitement au controllers
2/ Il faut faire le menage dans la requete pourque celle-ci soit interpretée correctement par index.php

Pour cela il te manque un fichier .htaccess. Oui, il ne sert pas uniquement a restreindre l'accès a des ressources tu peux aussi l'utiliser les 
RewriteEngine On

Point 2
# dynamically setup base URI
RewriteCond %{REQUEST_URI}::$1 ^(/.+)/(.*)::\2$
RewriteRule ^(.*) - [E=BASE_URI:%1]

Point 1
# redirect every request to index.php
# and give the relative URL in "_url" GET param
RewriteCond %{REQUEST_FILENAME} !-d
RewriteCond %{REQUEST_FILENAME} !-f
# rediriger TOUTES les requêtes qui mène au dossier dans lequel se trouve le .htaccess vers index.php
# appeler /products => index.php?_url=/products
RewriteRule ^(.*)$ index.php [QSA,L]