# Constat
J’ai tenté de faire fonctionner ton appli et voila ce qu’il s’est passé :

# Etape1
Lorsque je tape l'adresse : http://localhost/apprenant4/public/
1/ le message suivant apparait en haut de la page
   Fatal error: Uncaught Error: Class "\App\Controllers\ErrorController" not found in C:\laragon\www\apprenant4\vendor\benoclock\alto-dispatcher\Dispatcher.php:124 Stack trace: #0 C:\laragon\www\apprenant4\public\index.php(190): Dispatcher->dispatch() #1 {main} thrown in C:\laragon\www\apprenant4\vendor\benoclock\alto-dispatcher\Dispatcher.php on line 124
   J'ai été dans \App\Controllers et effectivement il n'y a pas de ErrorController. Et là ton autoloader n'a pas récupérer un class qui n'est pas écrite
   Bon normalement tu n'es pas censé aller sur une page d'erreur donc voyons pourquoi tu ne match pas avec une route.

Déjà il manque le .htaccess, car la variable match de ton index.php renvoie false 

# Etape2
Après avoir ajouté le .htaccess j'ai toujours le même message d'erreur
Quand je vais un var_dump sur la variable match j'obtiens :
array(3) { ["target"]=> array(2) { ["method"]=> string(4) "home" ["controller"]=> string(30) "App\Controllers\MainController" } ["params"]=> array(0) { } ["name"]=> string(9) "main-home" }
Et là je remarque que MainController n'existe pas dans App\Controllers, ça ne peut donc pas fonctionner

# Etape3
Je teste une autre adresse en me basant sur tes routes par exemple
http://localhost/apprenant4/public/teachers/list puisque c'est le controller TeachersController (qui existe) qui est censé être appelé.
Je tombe sur le message suivant : 
Parse error: syntax error, unexpected identifier "teachers" in C:\laragon\www\apprenant4\App\Controllers\TeachersController.php on line 20
Il y avait à cet endroit l'appel à la fonction teachers() que j'ai retiré

# Etape4
 Après réexécution j’ai un nouveau message
        Fatal error: Uncaught Error: Class "App\Controllers\CoreController" not found in C:\laragon\www\apprenant4\App\Controllers\TeachersController.php:8
Et en effet ta classe class TeachersController extends CoreController et CoreController n'existe pas.
J'imagine que tu voulais créer une classe parente qui mettrai en commun toutes les fonctionnalités communes aux différents controller pour factoriser ton code
#Etape5
J'ai enlevé l'extends mais j'ai encore d'autres erreurs.

# Conclusion
Honnêtement je vais m'arrêter là, il me semble qu'il serait plus productif de reprendre ensemble les concepts de MVC, mais aussi de class, d'héritage, de variables de fonctions etc. Il faut qu’on voie comment tu peux raccrocher les wagons pour ne pas décrocher.
