# Constat

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

# Que faire
## La feuille de style
Les erreurs de type 404 signifie qu'une ressource n'est pas trouvé à l'endroit indiqué.
par exemple ton fichier de vue ici header.tpl.php contient le liens suivant  
    <!-- We can still have our own CSS file -->
    <link rel="stylesheet" href="./css/style.css">

## Le Routage

### La cause
cela vient de ta variable $match d'index.php qui contient les routes à la valeur FALSE. Cela signifie qu'il ne trouve pas la route en question.
La question est pourquoi ?
Parceque tes routes enregistrées sont :
/public /teachers /students/list /teachers/add /students/add /signin /logout /403 /404 /appusers
mais les requetes envoyées sont prefixes par /apprenant1 et suffixe par un / exemple /apprenant1/public/
Il faut donc:
1/ Rediriger toutes le requetes sur index.php qui sert de front controller et qui va recuperer les requetes et deleguer le traitement au controllers
2/ Faire le menage dans la requete pourque celle-ci soit interpretée correctement par index.php

### Solution
j'ai donc ajouté le .htaccess pour 
1/ Rediriger toutes les requêtes sur index.php qui sert de front controller et qui va récupérer les requêtes et déléguer le traitement au controllers
2/ Faire le ménage dans la requête pourque celle-ci soit interprétée correctement par index.php
Pour cela il te manque un fichier .htaccess. Oui, il ne sert pas uniquement a restreindre l'accès a des ressources tu peux aussi l'utiliser les 
RewriteEngine On

#### Point 1
Rediriger TOUTES les requêtes vers index.php et donner un URL relative URL dans "_url" GET param
RewriteCond %{REQUEST_FILENAME} !-d
RewriteCond %{REQUEST_FILENAME} !-f

Rediriger TOUTES les requêtes qui mène au dossier dans lequel se trouve le .htaccess vers index.php appeler /products => index.php?_url=/product
RewriteRule ^(.*)$ index.php [QSA,L]

#### Point 2
dynamically setup base URI
RewriteCond %{REQUEST_URI}::$1 ^(/.+)/(.*)::\2$
RewriteRule ^(.*) - [E=BASE_URI:%1]

# Les bons points
Ton code est assez complet, tu utilises l'heritage pour tes controllers
Tu encrypte tes mots de passes avec un algorithme asymetriques

# Les ameliorations
Tu peux faire encore mieux en :
Découplant ton code. Sache que dans l’idéal qu'une classe ne doit avoir qu’une seule responsabilité. Ainsi dans ont index.php tu pourrais déléguer la création des routes
Dans ton controller CoreController tu peux ajouter d'autre fonctionnalités communes, comme la gestion des token csrf, la gestion des redirections vers la page d'accueil ou de login lorsqu'un utilisateur n'a pas le rôle nécessaire.
Si tu veux aller plus loin tu peux ajouter le blocage des login après plusieurs tentatives infructueuses ou ajouter un captcha, tu peux aussi ajouter le renouvellement du mot de passe.

# Conclusion
Dans l'ensemble c'est un bon travail, continues.

