
Voici une version corrigée et réorganisée de votre texte :

Projet GitHub - Résumé
Ce projet est divisé en deux catégories : la partie algorithmique et la partie web, où les résultats et les conclusions de la première partie sont affichés.

Partie Algorithmique :
L'objectif principal était de permettre à l'ordinateur de traiter une image et de la reconstruire. Cela signifie qu'il doit être capable de recréer une image donnée, à partir des données récupérées lors de la présentation de l'image initiale. De plus, nous cherchons à réaliser diverses opérations sur l'image, telles que l'obtention du squelette de l'image et d'autres manipulations.

Pour simplifier cette tâche, nous avons décidé d'utiliser des images en noir et blanc, bien que cela fonctionne également avec des images en nuances de gris.

Première Étape : La Carte des Distances Euclidiennes au Carré

Pour enregistrer les données de l'image, nous devons d'abord trouver un moyen approprié. Étant donné que nous travaillons avec des images en noir et blanc, les données sont simples : chaque pixel est soit blanc, soit noir. Bien qu'il soit tentant d'utiliser un tableau à deux dimensions pour cela, nous avons opté pour une approche plus sophistiquée, car nous prévoyons d'effectuer des opérations plus complexes ultérieurement.

Nous devons également enregistrer la distance entre un pixel de la forme et un pixel de l'arrière-plan. Pour ce faire, nous utilisons la carte des distances euclidiennes au carré. Cette carte remplace chaque pixel de l'image par la distance euclidienne au carré entre ce pixel et le pixel de l'arrière-plan le plus proche.

Deuxième Partie : Les Boules Maximales

Plutôt que de parcourir une liste de pixels pour redessiner la forme, nous avons opté pour une approche basée sur des boules maximales. En observant la carte des distances euclidiennes au carré, nous avons réalisé que toutes les formes pouvaient être représentées par des cercles remplis. Cette méthode consiste à garder uniquement les boules maximales, c'est-à-dire celles qui ne sont pas entièrement contenues dans d'autres boules. Nous avons optimisé ce processus en observant que la plupart des centres des boules maximales se trouvent sur l'axe médian de l'image.

Partie Web :
Pour présenter nos résultats de manière claire, nous avons décidé de les afficher sur une page HTML. Vous trouverez dans le dossier "Web" un fichier HTML à ouvrir pour consulter les résultats.

Cependant, nous avons également réalisé une deuxième version de la partie web, entièrement en JavaScript. Nous avons mis tous nos efforts pour créer un site élégant, dynamique et interactif. Vous pouvez y accéder via ce lien : Nom du site.

Pour plus de détails sur les conceptions HTML et JavaScript, une vidéo explicative est disponible ici : Lien vidéo.
