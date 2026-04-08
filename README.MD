# <p align="center">Macros Excel permettant d'exploiter facilement un export CSV issu du plugin dataexport</p>
---
Le plugin **dataexport** permet d'exporter l'historique de différentes commandes jeedom dans un fichier CSV. Cependant, le format des données exportées n'est pas très facile à manipuler :  
Par exemple, si on exporte plusieurs commandes pour une période donnée, le fichier généré contient d'abord l'historique de la première commande, puis celui de la seconde commande, ...  
Ce n'est pas très exploitable dans Excel, pour créer des graphiques par exemple.  
  
Le document excel proposé ici, **'jeedom_import_modele.xlsm'**, comporte un ensemble de macros permettant de faciliter le processus de mise en forme des données, et de génération de graphiques.  
Il a été développé avec un excel 2010 ... on peut supposer que c'est compatible avec une version plus récente d'excel.  
Vous trouverez également deux autres documents : **'marstek_2026-04-02.csv'** et **'marstek_2026-04-02.xlsm'**.  
Le premier est un fichier CSV généré par le plugin dataexport, concernant des commandes jeedom relatives à une batterie Marstek Venue E. 
Le second est une copie du fichier 'jeedom_import_modele.xlsm', après importation du fichier CSV et traitement à l'aide des macros proposés dans le modèle xlsm.  
 Ceci permet d'évaluer ce qu'il est possible de faire avec ce modèle ; je détaille en fin de ce README la procédure pour arriver au document final.  

A noter que tout le code a été généré par une IA (en l'occurence, gemini) ; ca a nécessité de nombreux aller-retours. Je n'ai pas de compétences excel avancé ni VBA.

Voici les fonctionnalités proposées :

## menu principal
Le menu permet d'accéder aux différents options possibles.  
C'est un menu flottant, que l'on peut positionner ou l'on veut.  
On peut activer l'affichage de ce menu de deux manières :  
- en cliquant sur l'onglet 'Compléments', puis sur 'Analyse Jeedom' et 'Ouvrir le menu de commandes'
- ou plus simplement avec le clavier : Ctrl-m (m comme menu)

Voici maintenant les différentes options de ce menu :

## Importer CSV Jeedom
Permet d'importer un fichier CSV provenant du plugin dataexport.  
Une fenêtre permet de choisir le fichier CSV à importer ; le dossier par défaut est celui depuis lequel on a lancé le document macro excel.  
L'importation depuis ce menu fait déja un premier traitement : il crée une feuille par commande incluse dans le fichier CSV ; le nom de la feuille est celui de la commande jeedom.

## Générer une feuille datas
Génère une nouvelle feuille dans le document. Cette feuille comporte en colonne A le 'timestamp' (date, heure, minutes, secondes) de l'échantillonnage ; et ensuite autant de colonnes que de commandes jeedom importées.  
Ce format permet ensuite une exploitation facile des informations ; en particulier, la génération de graphiques.  
On peut choisir le nom de la feuille générée ('datas' par défaut), la liste des commandes jeedom à traiter (par défaut, toutes les commandes incluses dans l'import CSV) et différentes options.
 il est conseillé de laisse les options validées.  
En particulier, les options "Activer le réchantillonnage', 'Remplir les vides', et le choix de rééchantillonage.  
Avec ces options, il y aura une ligne par pas d'échantillonnnage ; par exemple, si on choisi un pas d'1 mn, il y aura une ligne par minute précise. La valeur de chaque commande sera celle de la minute précise, s'il y en a une, ou la valeur immédiatement précédente.  
Ceci permet de faciliter le génération des graphes.

## Mettre un filtre sur date
Permet de limiter l'étendue de la feuille 'datas' courante.  
L'export CSV se fait jour par jour. Dans l'exemple proposé (marstek_2026-04-02.xlm), l'export porte sur 2 jours pleins : les 2 et 3 avril. Mais la tranche horaire qui m'intéressait allait du 02/04 à 08h00 au 03/04 à 08h59 ; j'ai donc précisé des 2 limites pour que la feuille 'datas' soit tronquée comme désiré.

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
- le ou les colonnes concernant la partie gauche, et le ou les colonnes concernant la partie droite
- l'unité pour le coté gauche, de l'unité pour le coté droit : %, V, W, kWh, ...
- une case à cocher pour ajouter un quadrillage vertical ; et dans ce cas, le pas, en minutes
- les heures/minutes de début et de fin. Par défaut, ce sont les heures de début et de fin de la fauille 'datas'  

On peut ensuite re-travailler 'à la main' les graphiqes dans Excel ; par exemple, pour personnaliser les échelles.

## Masquer des séries d'un graphique
Parfois, on peut souhaiter masquer (ne pas afficher) une ou des séries d'un graphique.  
Cette fonction permet d'éliminer une ou plusieurs courbes à l'affichage, sans changer les autres options.  
C'est très simple à utiliser. 

## Créer un graphique zoom
Lorsqu'un graphique est affiché, on peut avoir l'envie de faire un 'zoom' sur une tranche horaire de celui-ci.  
On pourrait créer un nouveau graphique, avec les même options que les précédentres en changeant les heure/minutes de début et de fin.  
Mais si on a personnalisé ce graphique, ca demande du travail.  
Cette option permet de faire un zoom sur le graphique courant. 
Le titre par défaut est le titre initial, avec l'extension '_zoom'  
Il suffit de choisir les heure/minutes de début et de fin, puis le pas pour l'abscisse.  
On obtiendra une feuille identique à la précédente, avec la fourchette de heure/minute désirée.  
Le nom de la feuille sera celui de la feuille initiale, avec l'extension '_Zoom'

## Dupliquer un graphique
Permet de cloner un graphique.  
Ca n'a plus beaucoup d'utilité, on peut faire la même chose avec le menu  de création d'un zoom

## Actualiser un/des graphiques
L'objectif est de construire 'une bonne fois' un document excel complet, avec de nombreux graphiques, et de pouvoir ensuite de réutiliser sur les données d'un autre jour.  
Il suffit de copier le document excel initial, puis de faire un nouvel import CVS, puis de générer à nouveau une feuille datas, de même nom que la précédente.  
Ensuite, cliquer sur l'option 'Actualiser un/des graphiques' ; choisir le ou les graphiques à actualiser, et choisir les nouvelles heures de début et de fin.

## Activer / Désactiver bulles infos
A faire lors de la consultation d'un graphique.  
Si on clique une première fois sur ce bouton, on active les infos bulles.  
Il suffit maintenant de cliquer sur un point d'une courbe ; une info bulle affiche alors l'heure exacte, et la valeur des différentes séries présente sur le graphique.  
On peut ensuite cliquer sur un autre point pour raffraichir l'info bulle.  
Cliquer à nouveau sur le menu 'Activer / Désactiver bulles infos' ; ceci désactivera l'option pour la feuille courante.

## Nettoyer le classeur
Cette option permet de supprimer toutes les feuilles qui ne sont pas des graphiques ou des feuilles de type 'datas'.

---
# Etapes pour générer un fichier similaire à 'marstek_2026-04-02.xlsm' à partir de 'marstek_2026-04-02.csv'
---
## 1) Copie du modèle
Copie de 'jeedom_import_modele.xlsm' vers 'marstek_2026-04-02.xlsm'.

## 2) Importation du fichier 'marstek_2026-04-02.xlsm'
Ouvrir le menu : Ctrl-M  
Cliquer sur 'Importer CSV Jeedom' et choisir le fichier 'marstek_2026-04-02.xlsm'  

## 3) Générer la feuille 'datas'
Cliquer sur 'Générer une feuille datas' ; laisser toutes les options par défaut, et cliquer sur le bouton 'Générer'

## 4) Limiter la plage horaire d'observations
Se positioneer sur la feuille 'datas'.  
Cliquer sur 'Mettre un filtre sur dates'.  
Laisser les options par défaut, sauf les heures de début et de fin.  
- heure de début : 02/04/2026 08:00
- heure de fin : 03/04/2026 08:59  
Cliquer sur le bouton 'Appliquer le filtre'

## 5) Ajout d'une colonne 'moyenne'
Je souhaite avoir un colonne qui contienne la moyenne des tensions des 16 cellules de la batterie.  
Cliquer sur 'Ajouter une colonne calculée'  
- Nom de la colonne : moyenne
- Colonnes pour le calcul : cocher 'voltage cel 01' à 'voltage cel 16'
- Opération : cocher 'MOYENNE'  
Cliquer sur le bouton 'Ajouter'

## 6) Ajout de 2 graphiques
On va laisser les heures de début et de fin par défaut ; ce sont celles de la feuille 'datas' courante.  
Pour tous les graphiques générés ici, on va choisir en champs gauche la colonne 'batt SoC', et en unité coté gauche le caractère '%'. Ce champ correspond au pourcentage d'énergie estimée de la batterie par rapport à la capacité théorique maximale.

Il y aura donc, pour tous les graphiques, le SoC tracé, avec son échelle en pourcentage en partie gauche.

### 6.1) Daily Charging
On veut un graphe qui donne, en plus du SoC, le total d'énergie chargée et déchargée pour la journée. On choisi donc pour la partie droite les champs 'daily charging energy' et 'daily discharging energy' ; et comme unité 'kWh'  
Comme l'amplitude horaire est assez importante, on va choisir 60 pour le 'pas d'abscisse' ; donc un trait vertical toutes les 60 mn.
Le titre : 'Daily Charging'  
Cliquer sur le bouton 'Générer'  
On renomme la feuille générée 'Daily_Charging'  
L'échelle proposée va de '0, kWh', '0,5 kWh', ... '4, kWh'. Pas très joli à mon gout. 
Faire un clic droit sur l'axe vertival droit, choisir 'mise en forme de l'axe' 
Dans 'Nombre', le code de format courant est '#\ ##0,###\ " kWh"' ; remplacer par '#\ ##0,0\ " kWh"'  
L'échelle sera maintenant '0,0 kWh', '0,5 kWh', ... '4,0 kWh'

### 6.2) Cell Voltage
On veut également un graphe qui donne, en plus du SoC, la tension maximale et minimale des cellules (on pourait y ajouter la moyenne pcécédente).  
Pour la partie droite, on choisi les champs 'max cell voltage' et 'min cell voltage', et l'unité 'V'.  
'pas d'abscisse' : 60.  
titre : 'Cell Voltage'  
Cliquer sur le bouton 'Générer'  
On renomme la feuille générée 'Cell_Voltage'  

## 7) faire un zoom sur les deux graphes précédents
On souhaite zoomer, pour les 2 graphes, sur la période '02/04/2026 12:00' jusque '02/04/2026 19:59' ; avec un pas d'abscisse de 30mn.  
Il suffit, pour chaque graphe, de cliquer sur le menu 'Créer un graphique zoom', et d'indiquer les paramètres désirés.  

Pour 'Cell_Voltage_Zoom', on peut adapter l'échelle droite :  
Par défaut, elle va de 3.15V à 3,65V ; on va réduire de 3,3V à 3,6V.  
Dans 'Options d'axe' :
- Minimum : cocher 'Fixe', et choisir 3,3
- Maximum : cocher 'fixe', et choisir 3,6

## 8) Ajout d'un graphique contenant les infos de tension des 16 cellules
On souhaite ajouter un graphique contenant les infos de tension des 16 cellules.  
Ce qui intéresse, c'est la période 'zoom' : de 02/04/2026 12:00' jusque '02/04/2026 19:59'.  
On va donc créer un nouveau graphique, comme en paragraphe 6.2, mais avec cette plage horaire, en choisissant les champs 'voltage cel 01' à 'voltage cel 16'  
On choisi le titre 'All_Cell_Voltage', et le nom de la feuille 'All_Cell_Zoom'.  
On applique ensuite les mêmes modifications d'option de graphes que précédemment.  

Ce dernier graphique est très chargé : il affiche le SoC et les courbes de tension des 16 celleules.  

## 9) détermination des cellules qui divergent le plus
on se positionne sur le graphique 'All_Cell_Zoom' ete on valide l'option de menu 'Masquer des séries d'un graphique'.  
On peut maintenant masquer certaines séries. En procédant par étapes, on contate :
- que les cellules qui divergent le plus vers le haut sont la 08 puis la 10.
- que les cellules qui divergent le plus vers le bas sont la 02 puis la 07.
- et que les autres cellules ont pratiquement le même comportement.
