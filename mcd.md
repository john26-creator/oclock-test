# MCD

Ton entité USER peut liker un ou plusieurs PRODUCT.
Cela signifie qu'il y a une CIM (contrainte d'integrité multiple) entre USER et PRODUCT.
Dans ce cas les cardinalités de l'association LIKE sont du type 0/1-n. 
En effet un USER peut liker 0 ou 1 ou plusieurs PRODUCT et un PRODUCT peut etre liké par 0 ou 1 ou plusieurs USER

Autre remarque
Ton USER a au moins une ADDRESS sinon comment lui livrer un PRODUCT.
Si tu conserve la cardinalité 0-n sache qu'il faudra ajouter l'adresse de livraison au moment de l'achat et non la selectionner

