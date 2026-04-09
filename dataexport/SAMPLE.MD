# Tutoriel pour générer un fichier similaire à 'sample.xls' à partir de 'sample.csv'
---
## préalable
Le fichier **'sample.csv'** a été généré par le plugin jeedom **dataexport**, à partir de données provenant d'une batterie Marstek Venus E version 3.  
Il contient l'historique de 23 commandes (mesures) relevées toutes les minutes à l'aide du plugin mymodbus.  
On souhaite produire un fichier excel facilement exploitable, et contenant différents graphiques.  

On utilise le modèle **'jeedom_import_modele.xlsm'** pour cela.

Le produit final est le fichier **'sample.xlsx'** ; **'sample.xlsm'** est un fichier intermédiaire, idemtique au précédent, mais contenant les macros permettant de réaliser ce travail.

Voici la procédure suivie pour produire **'sample.xlsx'** :

## 1) Copie du modèle
Copie de 'jeedom_import_modele.xlsm' vers 'sample.xlsm'.

## 2) Importation du fichier 'sample.csv'
Ouvrir le menu : Ctrl-M  
Cliquer sur 'Importer CSV Jeedom' et choisir le fichier 'sample.csv'  

## 3) Générer la feuille 'datas'
Cliquer sur 'Générer une feuille datas' ; laisser toutes les options par défaut, et cliquer sur le bouton 'Générer'

## 4) Limiter la plage horaire d'observations
Se positionner sur la feuille 'datas'.  
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

## 6) Ajout d'une colonne 'diff_volt'
Je souhaite ajouter une colonne qui contienne la différence entre 'max cell voltage' et 'min cell voltage' ; c'est donc la différence entre la cellule ayant la tension maximale avec celle ayant la tension minimale.  
Cliquer sur 'Ajouter une colonne calculée'  
- Nom de la colonne : diff_volt
- Colonnes pour le calcul : cocher 'max cell voltage' et 'min cell voltage'
- Opération : cocher 'DIFFERENCE'  

Cliquer sur le bouton 'Ajouter'

## 7) Ajout de 2 graphiques
On va laisser les heures de début et de fin par défaut ; ce sont celles de la feuille 'datas' courante.  
Pour tous les graphiques générés ici, on va choisir en champs gauche la colonne 'batt SoC', et en unité coté gauche le caractère '%'. Ce champ correspond au pourcentage d'énergie actuelle estimée de la batterie par rapport à la capacité théorique maximale.

Il y aura donc, pour tous les graphiques, le SoC tracé, avec son échelle en pourcentage en partie gauche.

### 7.1) Daily Charging
On veut un graphe qui donne, en plus du SoC, le total d'énergie chargée et déchargée pour la journée. On choisi donc pour la partie droite les champs 'daily charging energy' et 'daily discharging energy' ; et comme unité 'kWh'  
Comme l'amplitude horaire est assez importante, on va choisir 60 pour le 'pas d'abscisse' ; donc un trait vertical toutes les 60 mn.
Le titre : 'Daily Charging'  
Cliquer sur le bouton 'Générer'  
On renomme la feuille générée 'Daily_Charging'  
L'échelle proposée va de '0, kWh', '0,5 kWh', ... '4, kWh'. Pas très joli à mon gout. 
Faire un clic droit sur l'axe vertical droit, choisir 'mise en forme de l'axe'.  
Dans 'Nombre', le code de format courant est '#\ ##0,###\ " kWh"' ; remplacer par '#\ ##0,0\ " kWh"'  
L'échelle sera maintenant '0,0 kWh', '0,5 kWh', ... '4,0 kWh'

### 7.2) Cell Voltage
On veut également un graphe qui donne, en plus du SoC, la tension maximale et minimale des cellules (on pourait y ajouter la moyenne pcécédente).  
Pour la partie droite, on choisi les champs 'max cell voltage' et 'min cell voltage', et l'unité 'V'.  
'pas d'abscisse' : 60.  
titre : 'Cell Voltage'  
Cliquer sur le bouton 'Générer'  
On renomme la feuille générée 'Cell_Voltage'  

## 8) faire un zoom sur les deux graphes précédents
On souhaite zoomer, pour les 2 graphes, sur la période '02/04/2026 12:00' jusque '02/04/2026 19:59' ; avec un pas d'abscisse de 30mn.  
Il suffit, pour chaque graphe, de cliquer sur le menu 'Créer un graphique zoom', et d'indiquer les paramètres désirés.  

Pour 'Cell_Voltage_Zoom', on peut adapter l'échelle droite :  
Par défaut, elle va de 3.15V à 3,65V ; on va réduire de 3,3V à 3,6V.  
Dans 'Options d'axe' :
- Minimum : cocher 'Fixe', et choisir 3,3
- Maximum : cocher 'fixe', et choisir 3,6

## 9) Ajout d'un graphique contenant les infos de tension des 16 cellules
On souhaite ajouter un graphique contenant les infos de tension des 16 cellules.  
Ce qui intéresse, c'est la période 'zoom' : de 02/04/2026 12:00' jusque '02/04/2026 19:59'.  
On va donc créer un nouveau graphique, comme en paragraphe 6.2, mais avec cette plage horaire, et en choisissant les champs 'voltage cel 01' à 'voltage cel 16'  
On choisi le titre 'All_Cell_Voltage', puis le nom de la feuille 'All_Cell_Zoom'.  
On applique ensuite les mêmes modifications d'option de graphes que précédemment.  

Ce dernier graphique est très chargé : il affiche le SoC et les courbes de tension des 16 celleules.  

## 10) détermination des cellules qui divergent le plus
on se positionne sur le graphique 'All_Cell_Zoom' ete on valide l'option de menu 'Masquer des séries d'un graphique'.  
On peut maintenant masquer certaines séries. En procédant par étapes, on constate :
- que les cellules qui divergent le plus vers le haut sont la 08 puis la 10.
- que les cellules qui divergent le plus vers le bas sont la 02 puis la 07.
- et que les autres cellules ont pratiquement le même comportement.

## 11) Ajout d'un graphique contenant la divergence maximal des tensions
Toujours dans la période 'zoom'.  
On crée un nouveau graphique comme précédemment, en choisissant la plage horaire de 'zoom', et :  
- titre : Diff_Cell_Volts_Zoom
- champs partie gauche : 'max cell voltage' et 'min cell voltage'
- champs partie droite : 'diff_volt'
- unité des 2 cotés : 'V'
- pas d'abscisse : 30
- heures de début et de fin : de 02/04/2026 12:00' jusque '02/04/2026 19:59'  

On fait ensuite quelques ajustements de présentation, pour que la courbe diff_volt ressorte bien :
- clic droit sur les séries, et pour chacune, dans 'Mettre en forme une série' :
  - 'max cell voltage' : on laisse la couleur par défaut (bleu),  mais on passe le style du trait en pointillé
  - 'min cell voltage' : la couleur par défaut est rouge ; on la passe en vert clair, et en pointillé
  - 'diff_volt' : la couleur par défaut est vert clair ; on la passe en rouge
- clic droit sur l'échelle de droite, puis choisir la couleur rouge

## 12) créer une feuille excel 'simple', sans macro ni infos superflues
L'idée est de pouvoir proposer un fichier excel sans macros ni les informations initiales importées.  
C'est un fichier que l'on pourra proposer, par exemple, à un support technique.  
Dans 'sample.xlsm', choisit l'option de menu 'Nettoyer le classeur', puis valider.  
Ensuite, faire 'Enregistrer  sous', et choisir comme type 'Classeur Excel (*.xlsx)'.