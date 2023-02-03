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

#Que faire
## La feuille de style
Les erreurs de type 404 signifie qu'une ressource n'est pas trouvé à l'endroit indiqué.
par exemple ton fichier de vue ici header.tpl.php contient le liens suivant  
    <!-- We can still have our own CSS file -->
    <link rel="stylesheet" href="./css/style.css">

## Le Routage
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

## Point 1
rediriger TOUTES les requêtes vers index.php et donner un URL relative URL dans "_url" GET param
RewriteCond %{REQUEST_FILENAME} !-d
RewriteCond %{REQUEST_FILENAME} !-f

rediriger TOUTES les requêtes qui mène au dossier dans lequel se trouve le .htaccess vers index.php appeler /products => index.php?_url=/product
RewriteRule ^(.*)$ index.php [QSA,L]

## Point 2
dynamically setup base URI
RewriteCond %{REQUEST_URI}::$1 ^(/.+)/(.*)::\2$
RewriteRule ^(.*) - [E=BASE_URI:%1]

Je remarque aue ton controller CoreController n'est pas abstract 
L'authorisation de tes routes n'est pas complete pqr exemple 'main-home' => [] est commente en plus d'etre vide
Je ne vois nulle pqrt lq gestion des token CSRF


// SIGNIN
$router->map(
    'GET',
    '/signin',
    [
        'method' => 'signin',
        'controller' => $controllersNamespace . 'AppUserController'
    ],
    'appuser-signin'
);

"/apprenant1/public/"
 /apprenant1/public//public
 /apprenant1/public//teachers
 /apprenant1/public//students/list 
 /apprenant1/public//teachers/add
 /apprenant1/public//students/add
 /apprenant1/public//signin
/apprenant1/public//logout

array(3) { ["target"]=> array(2) { ["method"]=> string(6) "signin" ["controller"]=> string(34) "\App\Controllers\AppUserController" } ["params"]=> array(0) { } ["name"]=> string(14) "appuser-signin" }

$route
/public /teachers /students/list /teachers/add /students/add /signin /logout /403 /404 /appusers

$requestUrl
/apprenant1/public/ /apprenant1/public/ /apprenant1/public/ /apprenant1/public/ /apprenant1/public/ /apprenant1/public/ /apprenant1/public/ /apprenant1/public/ /apprenant1/public/ /apprenant1/public/

/signin : / | /signin : /teachers | /signin : /teachers/add | /signin : /students | /signin : /students/add | /signin : /appusers | /signin : /appusers/add | /signin : /signin | bool(true)


 ["routes":protected]=> array(22) { 
[0]=> array(4) { 
			[0]=> string(3) "GET" 
			[1]=> string(1) "/" 
			[2]=> array(2) { ["method"]=> string(4) "home" ["controller"]=> string(31) "\App\Controllers\MainController" }
			[3]=> string(9) "main-home" 
		   } 
[1]=> array(4) { 
			[0]=> string(3) "GET" 
			[1]=> string(9) "/teachers" 
			[2]=> array(2) { ["method"]=> string(4) "list" ["controller"]=> string(34) "\App\Controllers\TeacherController" } 
			[3]=> string(12) "teacher-list" 
		   }
[2]=> array(4) { [0]=> string(3) "GET" [1]=> string(13) "/teachers/add" [2]=> array(2) { ["method"]=> string(3) "add" ["controller"]=> string(34) "\App\Controllers\TeacherController" } [3]=> string(11) "teacher-add" } [3]=> array(4) { [0]=> string(4) "POST" [1]=> string(13) "/teachers/add" [2]=> array(2) { ["method"]=> string(7) "addPost" ["controller"]=> string(34) "\App\Controllers\TeacherController" } [3]=> string(16) "teacher-add-post" } [4]=> array(4) { [0]=> string(3) "GET" [1]=> string(23) "/teachers/[i:id]/delete" [2]=> array(2) { ["method"]=> string(6) "delete" ["controller"]=> string(34) "\App\Controllers\TeacherController" } [3]=> string(14) "teacher-delete" } [5]=> array(4) { [0]=> string(3) "GET" [1]=> string(16) "/teachers/[i:id]" [2]=> array(2) { ["method"]=> string(6) "update" ["controller"]=> string(34) "\App\Controllers\TeacherController" } [3]=> string(14) "teacher-update" } [6]=> array(4) { [0]=> string(4) "POST" [1]=> string(16) "/teachers/[i:id]" [2]=> array(2) { ["method"]=> string(10) "updatePost" ["controller"]=> string(34) "\App\Controllers\TeacherController" } [3]=> string(19) "teacher-update-post" } [7]=> array(4) { [0]=> string(3) "GET" [1]=> string(9) "/students" [2]=> array(2) { ["method"]=> string(4) "list" ["controller"]=> string(34) "\App\Controllers\StudentController" } [3]=> string(12) "student-list" } [8]=> array(4) { [0]=> string(3) "GET" [1]=> string(13) "/students/add" [2]=> array(2) { ["method"]=> string(3) "add" ["controller"]=> string(34) "\App\Controllers\StudentController" } [3]=> string(11) "student-add" }