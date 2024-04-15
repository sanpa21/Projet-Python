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
<p align="justify">
Afin d'accéder aux données du marché, nous avons fait usage de la bibliothèque yfinance qui récupère les données de Yahoo Finance. La bibliothèque permet d'accéder aux prix des actions, à des données historiques, et à des informations financières et statistiques. </p>
<p align="justify">
Nous avons également utilisé le module matplotlib.pyplot de la bibliothèque matplotlib qui est souvent utilisée pour créer des visualisations et des graphiques de données. Ce module permet notamment de créer des graphiques simples, personnaliser des graphiques ou encore les exporter. Aussi liée à la bibliothèque matplotlib, la bibliothèque mplfinance simplifie la création de graphiques financiers interactifs et personnalisables, en particulier des graphiques en chandeliers japonais, des graphiques en barres, des graphiques en lignes, etc...  </p>
<p align="justify">
Une autre bibliothèque très connue que nous utilisons est la bibliothèque pandas pour la manipulation et l'analyse de données. Elle offre des structures de données flexibles et performantes, ainsi que des outils pour lire, écrire, filtrer, regrouper, joindre et analyser des données tabulaires et de séries temporelles. </p>
<p align="justify">
NumPy (ou Numeric Python) est une bibliothèque également très connue, fondamentale pour le calcul numérique. Elle fournit des structures de données et des fonctions pour travailler efficacement avec des tableaux multidimensionnels ainsi que des outils pour effectuer des opérations mathématiques et statistiques sur ces tableaux. </p>
<p align="justify">
Enfin, la dernière bibliothèque que nous importons est os qui fournit des fonctionnalités pour interagir avec le système d'exploitation, telles que la navigation dans le système de fichiers, la manipulation des chemins de fichiers, la création et la suppression de répertoires. Cette bibliothèque offre une interface pour accéder aux fonctionnalités du système d'exploitation.</p>



  
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

### A. Les indicateurs de performance classique

#### 1. Rendements

##### i. Rendements de la stratégie

##### ii. Rendements cumulatifs

##### iii. Rendement moyen annuel

#### 2. Volatilité 

#### # i. Volatilité quoditienne

##### ii. Volatilité annuelle

#### 3. Bêta

### B. Les indicateurs de performance "moins" classiques

#### 1. Bêta haussier

#### 2. Bêta baissier

### 3. Drawdown Maximal

### 4. Kurtosis

### 5. Skewness

## IV. Visualiser les résultats de la stratégie : méthode **Plot**


## V. Tester le backtest 

<p align="justify">
Prenons un exemple simple de stratégie. Nous calculons la moyenne mobile sur 50 jours pour un actif et également la moyenne mobile sur 200 jours. Sachant que la moyenne mobile sert à identifier des tendances sur des périodes plus ou moins longues, nous comparons ces deux moyennes mobiles. Une moyenne mobile sur 50 jours (court terme) supérieure à la moyenne mobile sur 200 jours (long terme) peut indiquer une tendance à la hausse récente des prix. La stratégie consistera alors à acheter à ce moment et attendre que les prix continuent à monter sur le court terme (position = 1). L’actif sera revendu lorsque la moyenne mobile sur 200 jours sera supérieure à celle sur 50 jours car cela indique une tendance à la baisse à court terme, soit un risque de baisse de la valeur de l’actif dans les jours qui suivent (position = -1). </p>


