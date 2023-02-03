# Constat
1/ Lorsque je tape l'adresse : http://localhost/apprenant2/public/appuser/list.html
   Le message suivant apparait en haut de la page
    Warning: require_once(C:\laragon\www\apprenant2\app\Controllers/../views/error/err404.tpl.php): Failed to open stream: No such file or directory in C:\laragon\www\apprenant2\app\Controllers\CoreController.php


2/ Lorsque je tape l'adresse : http://localhost/apprenant2/public/teachers et que je souhaite modifier un professeur je n’ai pas les champs préremplis c'est dommage. 
L'ajout/modification ne fonctionne pas non plus le code n'a pas été écrit

3/ J'ai des erreurs similaires sur les autres onglets étudiants et utilisateurs la non plus le code n'a pas été écrit

4/ Je remarque la déconnexion ne redirige pas vers l'écran d'accueil ou de login. Pas de session gerée


Pense aussi à gérer la session. Au login il faut vérifier les identifiants de ton user et créer une session avec l'id de ton utilisateur (au moins)
Il faut que les utilisateurs aient un mot de passe chiffré avec un algorithme asymétrique tu type bcrypt ou sha512. Pour vérifier que ton utilisateur a le bon mot de passe il faudra comparer le mot de passe saisi avec l'empreinte de son équivalent en base de données

# Ce qu'il faut faire
1/ Il te manque la vue le dossier error avec le fichier /err404.tpl.php dedans, du coup ton controller appel un fichier qui n'existe pas
2/ Il serait intéressant de transmettre l'id du professionnel en variable GET. De cette façon pourrait dans la page de modification remplir les champs et surtout modifier le bon professeur en base de données.
L'url de page la de modification finie par add, cela porte à confusion, update serait un meilleur choix.
NB: les variables GET doivent être échappes de peur qu'une personne malicieuse injecte du javascript.
NB2: Pense aussi a ajouter un token csrf dans tes formulaires afin que l'on ne puisse pas utiliser ton identifiant de session pour effectuer des actions que tu ne veux pas faire
3/ Ecrire le code pour étudiant et utilisateurs
4/ Pour la session il faut utiliser session_start () en tout debut de fichier ensuite tu as accès a la super variable $_SESSION qui est un tableau qui référence toutes les variables en session
Pour la supprimer il faudra demander au controller de faire un session_destroy();. Ton navigateur conservera en l'id de session mais il ne sera plus référencé cote serveur.
Pour le login : Il faut que les utilisateurs aient un mot de passe chiffré avec un algorithme asymétrique tu type bcrypt ou sha512. Pour vérifier que ton utilisateur a le bon mot de passe il faudra comparer le mot de passe saisi avec l'empreinte de son équivalent en base de données
