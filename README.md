Ce projet est repartie de deux catégorie, la premiere c'est la parte algoritmique et la deuxieme c'est la partie web où l'on va afficher les resultats et les conclusions de la premiere partie.


Pour la partie algorithmique:

L'objectif de base était que l'ordinateur arrive à traiter une image et de la récréer. C'est à dire qu'en lui donnant une image, il arrive à la refaire seuelement grace à des données qui a recuperé quand on lui a montré la premiere image, mais pas seulement. On cherche à pouvoir réaliser des opérations sur sur l'image, comme obtenir le squelette de l'image et pleins d'autres.

Pour nous simplifier la tache on va utiliser des images en noir et blanc, ca fonctionne aussi avec des images en nuances de gris.


Premiere etape: la carte des distances euclidienne au carré

Nous devons en premier temps trouver un moyen pour que l'ordinateur enregistre les données de l'image. Etant donné qu'on travaille avec des images en noir et blanc, les données sont simples, soit un pixel est blanc, soit il est noir. Vous pourez me dire qu'on a juste a mettre dans un tableau à deux dimenssion à l'emplacement du pixel, 0 si c'est un pixel noir, et 1 si c'est un pixel blanc et vous aurez raison. Cependant, on souhaitait faire des choses plus complexes par la suite qui n'aurait pas été possible avec un simple tableau de ce genre.

Il nous faillait également enregistrer la distance entre un pixel de la forme et un pixel du fond. Exemple : imaginons une image avec un cercle noir, on est d'accord pour dire que plus un pixel est pres du bord, plus sa distance avec le fond de l'image (en blanc) est petite, et que si on prend le pixel au centre du cercle, on a le pixel qui a la plus grande distance avec le fond. Nous stockons toutes ses informations dans un tableau à deux dimmenssions qui a la meme longueur et largeur que l'image, avec 0 si le pixel est blanc (un pixel de fond) et X si c'est un pixel noir (un pixel de forme) (X correspond à la distance au entre le pixel de forme et le pixel de fond).

Pour mettre en place cette algorithme, nous allons utiliser la carte de distance euclidienne au carré. La carte de distance euclidienne au carré est une représentation d'une image où chaque pixel est remplacé par la distance euclidienne au carré entre ce pixel et le pixel de fond le plus proche. Ce point peut être le centre de l'image ou un point spécifique. La distance euclidienne au carré entre deux points (x1, y1) et (x2, y2) dans un espace bidimensionnel est calculée comme suit :

Distance euclidienne au carré = =(x2​ − x1​)^2 +(y2 ​− y1​)^2

En remplaçant chaque pixel de l'image par cette valeur, on obtient une carte où les valeurs élevées représentent les zones les plus éloignées du pixel de fond le plus proche, tandis que les valeurs plus faibles représentent les zones plus proches.

exemple image :

Dans notre projet, afin d'observer des différences de complexité et de temps de réalisation, on a créé deux fonctions pour calculer la carte des distances euclidienne au carrée, une brute force et une optimisée.

Une fois qu'on a notre tableau avec les distances euclidiennes au carrée, on a mis en place une fonction de visualisation du tableau, les pixels de fond reste blanc et les pixels de forme sont affichié en fonction de leur distance, plus un pixel est éloigné du fond plus sa couleur tendra vers le blanc, et plus le pixel est proche, plus sa couleur tendra vers le noir.

Exemple:

Image normale puis image traitée


Deuxieme partie : les boules maximales

Pour redessiner la forme, on ne voulait pas simplement faire un parcours de liste et dessiner un pixel blanc quand la valeur est 0 et un pixel noir quand la valeur et différente de 0. Il fallait alors trouver une autre solution.

En observant la carte des distances euclidiennes au carré et les représentations des images, on a remarqué qu'en réalité, toutes les formes peuvent etre dessiné avec des cerles remplies, qui ont pour rayons la racine carrée de la distance du pixel stocké dans le tableau de la carte des distances euclidienne au carré. C'est pour cela qu'on va utiliser la methode des boules maxiamale.

Cette methode conciste à garder uniquement les boules qui sont maximales, c'est à dire que la boule n'est pas entierement contenue dans une autre boule. Car si une boule est non maximale ca veut dire qu'une autre boule ou un ensemble de boule peuvent dessiner la meme surface que celci et donc, dans une optique d'optimisation, nous allons garder que les boules maximales pour afficher que les cercles vraiment utiles pour la reconstitution de l'image.

Pour savoir si une boule X est maximale, on parcours toutes les autres boules du tableau et si la distance entre le centre de la boule X et le centre de la boule parcourue additionné au rayon du cercle le plus petit et inferieur c'est que c'est qu'elle est inclus dans une autre boule et donc n'est pas maxiamle. Si apres le teste avec toutes les boules la boule X est toujours pas déclarée comme "non-maximale" c'est qu'elle est maximale. 
Exemple: des teste avec les rayons

Cependant, faire un parcours entier pour verifier si une seule boule est maximale n'est pas du tout optimisé, surtout s'il reste de nombreuse autres boules à tester. On a alors remarqué que la plupart des centres des boules maximale se trouvent au niveau de l'axe médian, c'est le squelette de d'image, les lignes blanches qu'on voit sur la représentation de la carte des distances euclidiennes au carré.
Exemple de l'axe median et des centres des cercles


C'est pour cela que dans la version optimisée, on commence par traiter les cercles où les points se trouvent au niveau de l'axe médian. Cela nous permet d'éliminer beaucoup plus rapidement les boules nons maximales et donc de faire des parcours de plus en plus rapide.

On sauvegarde ensuite les boules maximales dans une liste "Boule". "Boule" est une structure où l'objet possede une position x et y de son centre, ainsi que son rayon.

Si vous souhaitez avoir plus de precision sur la carte des distance euclidiennes au carré et les boules maximale, je vous mets à disposision la these ( de ma professeur) qui explique le sujet plus en detail: https://perso.liris.cnrs.fr/laure.tougne/theses_doctorants/these_Aurelie_leborgne.pdf


Partie 3 : reconstruction de l'image

Avec cette liste de "Boule" on peut ensuite tracer les cercles et on observe qu'on arrive bien a avoir la forme de l'image de depart:

Il nous reste plus qu'à remplir les cercles pour avoir la forme complete:


Pour ce qui est de la fidelité de la reproduction, elle est à 96%. On pense que les 4% manquants prouviennent du dpi (dot per itch) qui n'est pas le meme entre l'image d'origine et l'image obtenue apres toutes les opérations

Nous avons également ajouté des fonctions pour tracer des graphiques afin de voir les différences de temps de calcul, de resultat,... entre les methodes brute force et optimisé, et également si le bruit avait un impact sur le temps de calcul ou sur le résultat (le bruit est une mutation de l'image : exemple image sans bruit et image avec bruit)

Pour réalisé les graphiques nous avons utiliser le controle graphique "Chart".



Pour la partie web:

Afin de montrer nos résultats clairement, nous avons decider de les afficher sous d'une page html.

Vous trouverai dans le dossier Web, le fichier html à ouvrir pour voir la page ainsi que nos résultats (en francais).

Cependant, on cherchait à aller plus loins et à depasser nos limites, c'est pour cela que nous avons fait une deuxieme version de la partie web, uniquement en javascript. Nous avons mis tous les efforts qu'on possedait pour faire un site élégant, dynamique et intéractive, voici l'acces au site: https://ezwin.netlify.app

Si vous souhaitez avoir plus de précision sur la conception de la premiere version en html et la deuxieme version en javascript, j'ai réalisé une video où j'explique plus en detail le procedé ainsi que nos choix: https://youtu.be/zJ3VeK5o50Q?si=sYNVVdltUfIWi16q
