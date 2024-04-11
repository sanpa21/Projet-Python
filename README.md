# Projet Python L3 EIF : Sujet B, 3 personnes, Option A
SAN Pauline, SUAU Sarah, XIANG Séverine
## Introduction 

Remonter le temps pour mettre à l’épreuve une stratégie d’investissement : telle est l’essence du backtesting.
<p align="justify">
Fondé sur le premier principe de l’analyse technique, selon lequel “l’histoire se répète”, le backtesting est une méthode qui exploite les données historiques des marchés pour simuler et évaluer la performance d'une stratégie d'investissement. Son objectif est de fournir un aperçu de la manière dont cette stratégie aurait réagi dans les conditions de marché passées. </p>
<p align="justify">
Comparé à des tests basés sur des données simulées artificiellement, le backtesting est considéré comme plus fiable car il s'applique à des conditions de marché réelles, susceptibles de se reproduire. De plus, il aide à réduire les risques inhérents à une stratégie. Le Comité de Bâle sur le contrôle bancaire souligne d'ailleurs que le backtesting est un élément clé de l'approche du modèle interne en matière de gestion des risques de marché. </p>

<p align="justify">
Cependant, le backtesting présente plusieurs limites, notamment le biais de survie, qui peut fausser la perception de la performance en excluant les actifs ou stratégies qui ont échoué par le passé. De manière plus générale, il peut induire en erreur en laissant croire qu'une stratégie qui a fonctionné dans le passé sera tout aussi efficace à l'avenir, alors que les marchés évoluent constamment. </p>

<p align="justify">
Malgré ces limites, le backtesting demeure une pratique courante dans le domaine de la finance, largement utilisé par les investisseurs et les gestionnaires de portefeuille pour évaluer la viabilité de leurs stratégies d'investissement. 
</p>

<p align="justify">
Ainsi, dans le cadre de ce projet, nous allons développer un backtesteur, sur Jupyter, qui accepte une fonction de stratégie applicable à plusieurs actifs.
</p>

## I. Initialisation de la classe et importation des bibliothèques utilisées




  
## II. Récupération et stockage local des données : méthode **Compute**


<p align="justify">
La méthode <strong>Compute</strong> est conçue pour récupérer les données financières des marchés et des symboles financiers, spécifiés par l'utilisateur, sur une période donnée. Initialement, cette méthode lit les données à partir de fichiers CSV, s'ils existent, grâce à la fonction <strong>pd.read_csv()</strong>. Si ces fichiers CSV ne sont pas présents localement, la méthode utilise l'API de Yahoo Finance pour récupérer les données. Pour cela, on utilise la fonction <strong>yf.download()</strong>. Ces données sont ensuite stockées locelemnt dans des fichiers CSV. </p>

<p align="justify">
Les fichiers CSV sont nommés de manière à inclure le symbole ou le marché concerné, ainsi que les dates de début et de fin de la période.
Une fois les données récupérées et stockées localement, elles sont converties en Dataframe pour un traitement plus facile dans <strong>self.df_market</strong>, pour chaque marché, et dans <strong>self.df</strong>, pour chaque symbol lesquels sont stockés dans un dictionnaire <strong>self.symbols</strong>. </p>

<p align="justify">
Pour chaque marché dans <strong>self.df_market</strong>, deux colonnes supplémentaires sont ajoutées : une pour les rendements négatifs et une autre pour les rendements positifs, les deux ayant été obtenues en calculant les variations des prix de clôture par rapport au jour précédent puis les données positives et négatives sont récupérées grâce à la fonction <strong>np.where</strong>. </p>

<p align="justify">
Enfin, pour chaque symbole financier, une colonne supplémentaire est ajoutée, dans <strong>self.df</strong>, pour stocker les positions, après avoir appliqué la stratégie sur les données obtenues précédemment. </p>

## III. Mesurer la performance de la stratégie : méthode **Summary**

## IV. Visualiser les résultats de la stratégie : méthode **Plot**


## V. Tester le backtest 

<p align="justify">
Prenons un exemple simple de stratégie. Nous calculons la moyenne mobile sur 50 jours pour un actif et également la moyenne mobile sur 200 jours. Sachant que la moyenne mobile sert à identifier des tendances sur des périodes plus ou moins longues, nous comparons ces deux moyennes mobiles. Une moyenne mobile sur 50 jours (court terme) supérieure à la moyenne mobile sur 200 jours (long terme) peut indiquer une tendance à la hausse récente des prix. La stratégie consistera alors à acheter à ce moment et attendre que les prix continuent à monter sur le court terme (position = 1). L’actif sera revendu lorsque la moyenne mobile sur 200 jours sera supérieure à celle sur 50 jours car cela indique une tendance à la baisse à court terme, soit un risque de baisse de la valeur de l’actif dans les jours qui suivent (position = -1). </p>


