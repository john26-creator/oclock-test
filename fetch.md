#A Quoi sert le fetch ?

Tu as déjà vu en javascript l'utilisation du dom et tu as vu que tu pouvais manipuler la structure html, le contenu, le style, réaliser des animations.
Le fetch c'est une façon de communiquer avec un serveur. Grace a cette méthode magique tu vas pouvoir récupérer des données comme la météo, l'historique de cours boursier ect. Et les afficher par exemple dans des graphiques.
Bien sûr il faudra utiliser une bibliothèque pour cela.

#Comment ça fonctionne?
Tout d'abord sache qu'il peut passer un temps plus ou moins long entre le moment où tu fais ton fetch et le moment où tu reçois la réponse.
- Si tu veux continuer à exécuter le reste de ton code en attendant la réponse on dit que tu fais de l'asynchrone.
- Si tu veux attendre la réponse avant d'exécuter le reste de ton code en attendant la réponse on dit que tu es en mode synchrone.

#Comment on l'utilise ?
En mode asynchrone nous allons utiliser le mot clef then signifie ensuite/alors. Dans l'exemple ci-dessous nous allons chercher une liste de films au format JSON.

fetch('http://example.com/movies.json') // cherchons les films
  .then((response) => response.json())  // quand la reponse arrivera alors executons la fonction (callback) et faisons en un objet JSON
  .then((data) => console.log(data));   // quand l'objet JSON sera renvoye alors ecrivons le resultat dans la console


En mode synchrone, nous allons utiliser le mot clef await qui signifie attendre. le code va donc attendre la fin de l'instruction avant de passer à la suivante

const response = await fetch('http://example.com/movies.json'); // on attend le retour du serveur
const movies = await response.json(); // puis on attend l'objet JSON
console.log(data);                    // avant d'afficher en console les filmes   

Voilà les bases, je te laisse creuser si tu le souhaites le fetch https://javascript.info/fetch-api. Tu verras il y a pleins de belles possibilités supplémentaires.