# Débogage de l'application

## Etape 1
### Scenario
Lorsque je tape l'adresse : http://localhost/apprenant3/public/
      Dans la console j'ai une erreur 404
      La page qui s'affiche est une 404 générées qui affiche
      Le texte "La ressource demandée n'existe pas..."
      Et un lien vers la page d'accueil
      Aux cliques sur le liens je vais sur lolahost/home ce qui me donne une erreur 404

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

## Etape 2
### Scenario
Le lien vers la page d'Accueil m'envoie sur http://localhost/apprenant3/public/home et affiche la page d'accueil
En cliquant sur l'onglet teacher je suis dirigé sur http://localhost/apprenant3/public/teacher_list et j'ai cette erreur

Erreur de connexion...
SQLSTATE[HY000] [1045] Access denied for user 'root'@'localhost' (using password: YES)
#0 C:\laragon\www\apprenant3\app\Utils\Database.php(42): PDO->__construct('mysql:host=loca...', 'root', 'root', Array)

### Solution
Je change donc le config.ini pour mettre mes identifiants de base de données et la page s'affiche correctement. C'est bien tu utilises le fichier de configuration pour la connexion

## Etape 3
En cliquant sur ajout je tombe sur la page http://localhost/apprenant3/public/student/add.
Le formulaire s'affiche
A l'ajout le message suivant s'affiche
Warning: PDO::exec(): SQLSTATE[HY000]: General error: 1364 Field 'teacher_id' doesn't have a default value in C:\laragon\www\apprenant3\app\Models\Student.php on line 39
string(38) "erreur, problème avec la requête SQL"

### Solution
Deux possibilités :
1/ Tu modifies ta table et tu mets ton id a AUTO_INCREMENT. Cela crée un séquence qui génère un id a l'ajout sans que tu aies a le renseigner
2/ Dans ton php tu récupères le dernier id inséré https://www.php.net/manual/en/pdo.lastinsertid.php et tu l'incrémente toi même avant de persister ton modèle en base
NB: nous devrions avoir student_id et non teacher_id
NB2: pas de token CSRF dans le formulaire
Il serait intéressant de garder le header contenant les onglets sur chaque page.
Le concept de template te permet de faire cela en formant tes pages par la composition de morceaux de template

##Le reste a faire
La modification, la suppression des étudiants et professeurs, la déconnexion et la connexion

### Solution
Pour la session il faut utiliser session_start () en tout début de fichier ensuite tu as accès a la super variable $_SESSION qui est un tableau qui référence toutes les variables en session
Pour la supprimer il faudra demander au controller de faire un session_destroy();. Ton navigateur conservera en l'id de session mais il ne sera plus référencé cote serveur.
Pour le login : Il faut que les utilisateurs aient un mot de passe chiffré avec un algorithme asymétrique tu type bcrypt ou sha512. Pour vérifier que ton utilisateur a le bon mot de passe

