# <p align="center">Macros Excel permettant d'exploiter facilement un export CSV issu du plugin dataexport</p>
---
Le plugin **dataexport** permet d'exporter l'historique de différentes commandes jeedom dans un fichier CSV. Cependant, le format des données exportées n'est pas très facile à manipuler :  
Par exemple, si on exporte plusieurs commandes pour une période donnée, le fichier généré contient d'abord l'historique de la première commande, puis celui de la seconde commande, ...  
Ce n'est pas très exploitable dans Excel, pour créer des graphiques par exemple.  
  
Le document excel proposé ici, **'jeedom_import_modele.xlsm'**, comporte un ensemble de macros permettant de faciliter le processus de mise en forme des données, et de génération de graphiques.  
Il a été développé avec un excel 2010 ... on peut supposer que c'est compatible avec une version plus récente d'excel.  

Vous trouverez également quatre autres documents : **'sample.csv'**, **'sample.xlsm'**, **'sample.xlsx'** et [**'SAMPLE.MD'**](SAMPLE.MD).  
- Le premier est un fichier CSV généré par le plugin dataexport, concernant des commandes jeedom relatives à une batterie Marstek Venue E. 
- Le second est une copie du fichier 'jeedom_import_modele.xlsm', après importation du fichier CSV et traitement à l'aide des macros proposés dans le modèle xlsm.  
- Le troisième est un fichier excel issu du précédent, et expurgé des macros et des feuilles directement liées à l'import CSV.   
- Le quatrième, [SAMPLE.MD](SAMPLE.MD), est un tuto indiquant la procédure pour obtenir le résultat à partir du fichier CSV original

 Ceci permet d'évaluer ce qu'il est possible de faire avec ce modèle. Le tuto SAMPLE.MD détaille la procédure pour arriver au document final.  

A noter que tout le code a été généré par une IA (en l'occurence, gemini) ; ca a nécessité de nombreux aller-retours. Je n'ai pas de compétences excel avancé ni VBA.

Voici les fonctionnalités proposées :

## menu principal
Le menu permet d'accéder aux différents options possibles.  
C'est un menu flottant, que l'on peut positionner ou l'on veut.  
On peut activer l'affichage de ce menu de deux manières :  
- en cliquant sur l'onglet 'Compléments', puis sur 'Analyse Jeedom' et 'Ouvrir le menu de commandes'
- ou plus simplement avec le clavier : **Ctrl-m** (m comme menu)

Voici maintenant les différentes options de ce menu :

## Importer CSV Jeedom
Permet d'importer un fichier CSV provenant du plugin dataexport.  
Une fenêtre permet de choisir le fichier CSV à importer ; le dossier par défaut est celui depuis lequel on a lancé le document macro excel.  
L'importation depuis ce menu fait déja un premier traitement : il crée une feuille par commande incluse dans le fichier CSV ; le nom de la feuille est celui de la commande jeedom.

## Générer une feuille datas
Génère une nouvelle feuille dans le document. Cette feuille comporte en colonne A le 'timestamp' (date, heure, minutes, secondes) de l'échantillonnage ; et ensuite autant de colonnes que de commandes jeedom importées.  
Ce format permet ensuite une exploitation facile des informations ; en particulier, la génération de graphiques.  
On peut choisir le nom de la feuille générée (**'datas'** par défaut), la liste des commandes jeedom à traiter (par défaut, toutes les commandes incluses dans l'import CSV) et différentes options.
 il est conseillé de laisse les options validées.  
En particulier, les options "Activer le réchantillonnage', 'Remplir les vides', et le choix de rééchantillonage.  
Avec ces options, il y aura une ligne par pas d'échantillonnnage ; par exemple, si on choisi un pas d'1 mn, il y aura une ligne par minute précise. La valeur de chaque commande sera celle de la minute précise s'il y en a une, ou la valeur immédiatement précédente.  
Ceci permet de faciliter le génération des graphes.

## Mettre un filtre sur date
Permet de limiter l'étendue de la feuille 'datas' courante.  
Avec le plugin dataexport, L'export CSV se fait jour par jour. Dans l'exemple proposé (sample.xlm), l'export porte sur 2 jours pleins : les 2 et 3 avril 2026. Mais la tranche horaire qui m'intéressait allait du 02/04 à 08h00 au 03/04 à 08h59 ; j'ai donc précisé ces 2 limites pour que la feuille 'datas' soit tronquée comme désiré.

## Ajouter une colonne calculée
Permet d'ajouter une nouvelle colonne à la feuille 'datas' courante, dont les valeurs seront un calcul relatif à d'autres colonnes.  
On spécifie le nom de la colonne à ajouter, les colonnes sur lesquelles on appliquera le calcul, et l'opération voulue.  
Les opérations possibles sont :
- MOYENNE
- MAX
- MIN
- SOMME

## Générer un graphiques
L'idée est de générer un graphique avec en abscisse le temps (colonne A de la feuille datas).  
Avec une ou plusieurs séries en partie gauche, partageant la même échelle ; et une ou plusieurs séries en partie droite, partageant une autre échelle.  
On saisi :
- le titre du graphique
- la ou les colonnes concernant la partie gauche, et la ou les colonnes concernant la partie droite
- l'unité pour le coté gauche, et l'unité pour le coté droit : %, V, W, kWh, ...
- une case à cocher pour ajouter un quadrillage vertical ; et dans ce cas, le pas, en minutes
- les heures/minutes de début et de fin. Par défaut, ce sont les heures de début et de fin de la fauille 'datas'  

On peut ensuite re-travailler 'à la main' les graphiqes dans Excel ; par exemple, pour personnaliser les échelles.

## Masquer des séries d'un graphique
Parfois, on peut souhaiter masquer (ne pas afficher) une ou des séries d'un graphique.  
Cette fonction permet d'éliminer une ou plusieurs courbes à l'affichage, sans changer les autres options.  
C'est très simple à utiliser. 

## Créer un graphique zoom
Lorsqu'un graphique est affiché, on peut avoir envie de faire un 'zoom' sur une tranche horaire de celui-ci.  
On pourrait créer un nouveau graphique, avec les même options que les précédentres en changeant les heures/minutes de début et de fin.  
Mais si on a personnalisé ce graphique, ca demande du travail.  
Cette option permet de faire un zoom sur le graphique courant. 
Le titre par défaut est le titre initial, avec l'extension '_zoom'  
Il suffit de choisir les heure/minutes de début et de fin, puis le pas pour l'abscisse.  
On obtiendra une feuille identique à la précédente, avec la fourchette de temps désirée.  
Le nom de la feuille sera celui de la feuille initiale, avec l'extension '_Zoom'

## Dupliquer un graphique
Permet de cloner un graphique.  
Ca n'a plus beaucoup d'utilité, on peut faire la même chose avec le menu  de création d'un zoom

## Actualiser un/des graphiques
L'objectif est de construire 'une bonne fois' un document excel complet, avec de nombreux graphiques, et de pouvoir ensuite le réutiliser sur les données d'un autre jour.  
Il suffit de copier le document excel initial, puis de faire un nouvel import CVS, puis de générer à nouveau une feuille datas, de même nom que la précédente.  
Ensuite, cliquer sur l'option 'Actualiser un/des graphiques' ; choisir le ou les graphiques à actualiser, et choisir les nouvelles heures de début et de fin.

## Activer / Désactiver bulles infos
A faire lors de la consultation d'un graphique.  
En cliquant une première fois sur ce bouton, on active les infos bulles pour le graphique courant ; en cliquant à nouveau, on désactive les infos bulles.  
Lorsque l'optin est active, il suffit maintenant de cliquer sur un point d'une courbe ; une info bulle affiche alors l'heure exacte, et la valeur des différentes séries présente sur le graphique.  
On peut ensuite cliquer sur un autre point pour raffraichir l'info bulle.  
On peut également cliquer sur un endroit quelconque de la feuille pour fermer l'info bulle ; mais ca ne désactive pas la fonction pour la feuille courante.

## Nettoyer le classeur
Cette option permet de supprimer toutes les feuilles qui ne sont pas des graphiques ou des feuilles de type 'datas'.
