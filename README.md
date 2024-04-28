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
Afin d'accéder aux données du marché, nous avons fait usage de la bibliothèque <strong>yfinance</strong> qui récupère les données de Yahoo Finance. La bibliothèque permet d'accéder aux prix des actions, à des données historiques, et à des informations financières et statistiques. </p>
<p align="justify">
Nous avons également utilisé le module matplotlib.pyplot de la bibliothèque <strong>matplotlib</strong> qui est souvent utilisée pour créer des visualisations et des graphiques de données. Ce module permet notamment de créer des graphiques simples, personnaliser des graphiques ou encore de les exporter. Aussi liée à la bibliothèque matplotlib, la bibliothèque <strong>mplfinance</strong> simplifie la création de graphiques financiers interactifs et personnalisables, en particulier des graphiques en chandeliers japonais, des graphiques en barres, des graphiques en lignes, etc...  </p>
<p align="justify">
Une autre bibliothèque très connue que nous utilisons, est la bibliothèque <strong>pandas</strong> pour la manipulation et l'analyse de données. Elle offre des structures de données flexibles et performantes, ainsi que des outils pour lire, écrire, filtrer, regrouper, joindre et analyser des données tabulaires et de séries temporelles. </p>
<p align="justify">
<strong>NumPy</strong> (ou Numeric Python) est une bibliothèque également très connue, fondamentale pour le calcul numérique. Elle fournit des structures de données et des fonctions pour travailler efficacement avec des tableaux multidimensionnels, ainsi que des outils pour effectuer des opérations mathématiques et statistiques sur ces tableaux. </p>
<p align="justify">
Enfin, la dernière bibliothèque que nous importons est <strong>os</strong> qui fournit des fonctionnalités pour interagir avec le système d'exploitation, telles que la navigation dans le système de fichiers, la manipulation des chemins de fichiers, la création et la suppression de répertoires. Cette bibliothèque offre une interface pour accéder aux fonctionnalités du système d'exploitation.</p>



  
## II. Récupération et stockage local des données : méthode **Compute**


<p align="justify">
La méthode <strong>Compute</strong> est conçue pour récupérer les données financières des marchés et des symboles financiers, spécifiés par l'utilisateur, sur une période donnée. Initialement, cette méthode lit les données à partir de fichiers CSV, s'ils existent, grâce à la fonction <strong>pd.read_csv()</strong>. Si ces fichiers CSV ne sont pas présents localement, la méthode utilise l'API de Yahoo Finance pour récupérer les données. Pour cela, on utilise la fonction <strong>yf.download()</strong>. Ces données sont ensuite stockées localement dans des fichiers CSV. </p>

<p align="justify">
Les fichiers CSV sont nommés de manière à inclure le symbole ou le marché concerné, ainsi que les dates de début et de fin de la période.
Une fois les données récupérées et stockées localement, elles sont converties en Dataframe pour un traitement plus facile dans <strong>self.df_market</strong>, pour chaque marché, et dans <strong>self.df</strong>, pour chaque symbole stocké dans un dictionnaire <strong>self.symbols</strong>. </p>

<p align="justify">
Pour chaque marché dans <strong>self.df_market</strong>, deux colonnes supplémentaires sont ajoutées : une pour les rendements négatifs et une autre pour les rendements positifs, les deux ayant été obtenues en calculant les variations des prix de clôture par rapport au jour précédent, puis les données positives et négatives sont récupérées grâce à la fonction <strong>np.where</strong>. </p>

<p align="justify">
Enfin, pour chaque symbole financier, une colonne supplémentaire est ajoutée, dans <strong>self.df</strong>, pour stocker les positions, après avoir appliqué la stratégie sur les données obtenues précédemment. </p>

## III. Mesurer la performance de la stratégie : méthode **Summary**

### A. Les indicateurs de performance classique

#### 1. Rendements
<p align="justify">
Tout d’abord nous avons déterminé le rendement d’un titre, soit le gain ou la perte générée par un investissement sur une période donnée. Il mesure la performance financière d’un titre ou d’une action en fonction de l’évolution du marché. C’est un indicateur essentiel à prendre en compte chez les investisseurs pour évaluer la performance d’un investissement et prendre des décisions financières. Il peut être calculé de différentes façons mais nous avons ici, choisi de le déterminer en fonction du cours de clôture de l’actif.  En utilisant la méthode .pct_change() de la bibliothèque pandas, nous calculons le pourcentage de changement entre chaque prix de clôture consécutif, nous permettant d’obtenir les rendements de l’actif pour chaque période de temps. Ces rendements sont ensuite stockés dans une nouvelle colonne, nommée “asset_returns” dans le dataframe.  </p>

##### i. Rendements de la stratégie
<p align="justify">
Ce sont les gains ou pertes générés par une stratégie d’investissement sur une période donnée. Ils peuvent être exprimés en pourcentage par rapport au capital. Les rendements de la stratégie pour chaque jour sont définis par rapport au cours de clôture, en multipliant les rendements du titre par la position précédente de la stratégie, le résultat étant enregistré dans la nouvelle colonne “strategy_returns” du dataframe. </p>



##### ii. Rendements cumulatifs
<p align="justify">
Les rendements cumulatifss représentent la somme totale des rendements ou des pertes obtenus sur une période, réinvestis ou non, indépendamment de la durée d’investissement. Il est exprimé en pourcentage et est tel que : 
(Valeur actuelle de l'action - Valeur d'achat) / Valeur d'achat de l'action
Nous effectuons le calcul des rendements cumulatifs à partir des rendements quotidiens de la stratégie pour chaque jour de trading, en les ajustant en termes de taux de croissance en les stockant dans une nouvelle colonne du dataframe afin d’ afficher le dernier rendement cumulatif à chaque fois, pour vérification. 
Ces rendements cumulatifs sont ensuite assignés à une nouvelle colonne dans le DataFrame pour chaque actif financier. Enfin, le dernier rendement cumulatif de chaque actif financier est extrait et ajouté à une liste pour une référence ultérieure. </p>


##### iii. Rendement moyen annuel
<p align="justify">
Il mesure le montant de rendement ou de perte d’un investissement ou d’une stratégie peut rapporter chaque année sur une période donnée. Cet indicateur est notamment utile dans la comparaison des performances annuelles sur différents intervalles. Pour le calculer, il ne nécessite que de la moyenne du rendement sur une période donnée et la durée de l’investissement, sachant que le nombre de jours de trading ans une année typique est environ de 252 jours. Cela permet de normaliser le rendement quotidien moyen  pour obtenir la formule suivante : Annualized Average Return=moyenne de la stratégie*252 </p>


#### 2. Volatilité 
<p align="justify">
La volatilité est une mesure statistique de la dispersion des rendements ou du prix d'un actif d'un jour à l'autre, autour du prix moyen. Plus la volatilité est élevée, plus l’investissement est risqué, en raison du fait que les prix peuvent être moins prévisibles. Elle est souvent calculée en utilisant l'écart-type des changements de prix ou du rendement quotidien.</p>

##### i. Volatilité quoditienne
<p align="justify">
La volatilité quotidienne représente la mesure de la variation quotidienne des rendements d’un investissement. Nous déterminons la volatilité quotidienne en utilisant l’écart-type du rendement annuel de l’actif, soit une mesure de la dispersion de ses valeurs, que l’on stocke dans le dictionnaire volatility en Python. </p>

##### ii. Volatilité annuelle
<p align="justify">
Par ailleurs, la volatilité annuelle mesure la variation du rendement d'un investissement sur un an. Elle est cruciale pour évaluer le risque associé à un investissement sur long terme. 
Puisque la volatilité décrit les changements sur une période de temps donnée, il nous suffit de multiplier la volatilité quotidienne trouvée précédemment par la racine carrée du nombre de périodes, qui est ici de 252 jours pour spécifier l’activité du marché financier sur une année.  </p>

#### 3. Bêta
<p align="justify">
Le bêta est un indicateur de la volatilité, de risque relatif d'un actif par rapport à un marché de référence. Il indique dans quelle mesure le prix d’un actif réagit aux mouvements du marché de référence. Un bêta supérieur à 1 signifie que l'actif est plus volatile que le marché, tandis qu'un bêta inférieur à 1 indique un risque moindre par rapport au marché.
Nous prenons ici en compte la valeur de clôture dans les calculs des Bêta. En utilisant la fonction nb.cov de la bibliothèque NumPypour calculer la covariance entre les rendements de l’actif financier (action_returns) et ceux du marché (marche_returns), nous divisons ensuite cette covariance par la variance des rendements du marché pour obtenir le coefficient bêta. Ce dernier est enregistré dans un dictionnaire sous la clé correspondant au symbole de l’actif. </p>

### B. Les indicateurs de performance "moins" classiques

#### 1. Bêta haussier
<p align="justify">
Le Bêta haussier mesure la sensibilité d’un actif aux mouvements positifs du marché, et indique à quel point il est réactif aux hausses du marché, en tant que coefficient de volatilité. 
Nous calculons le Bêta haussier par rapport à chaque marché, mais à la différence du Bêta calculé précédemment, nous n'utilisons que les rendements positifs du marché dans le calcul de la covariance, en supposant que les rendements positifs du marché représentent des phases haussières du marché. Sinon, le calcul reste similaire à celui du Bêta. Le bêta haussier est calculé par le rapport entre la covariance des rendements de l'actif financier avec les rendements haussiers du marché, représentés par cov_haussier, et la variance des rendements haussiers du marché, représentée par var.
En affectant au marché un Bêta de 1 : par exemple si un actif a un bêta haussier de 1.2 alors il sera 20% plus réactif que le marché lorsque celui-ci monte, tandis qu’avec un bêta haussier de 0.8, l’actif sera alors 20% moins réactif que le marché, donc moins volatile que ce dernier. </p>


#### 2. Bêta baissier
<p align="justify">
Le Bêta baissier, quant à lui, est également un coefficient de volatilité, mesurant la sensibilité d'un actif financier aux mouvements négatifs du marché et indique à quel point il réagit aux baisses du marché. 
A la différence du Bêta haussier, nous utilisons cette fois les rendements négatifs du marché dans le calcul de la covariance en supposant que les rendements négatifs représentent des phases baissières, dans le calcul du Bêta baissier. Sinon, le reste du calcul reste similaire à celui du Bêta et du Bêta haussier. 
Il utilise la covariance des rendements de l'actif financier avec les rendements baissiers du marché, représentée par cov_baissier, et la divise par la variance des rendements baissiers du marché, représentée par var.
En affectant au marché un Bêta de 1 : par exemple si un actif a un bêta baissier de 1.2 alors il sera 20% plus réactif que le marché lorsque celui-ci baisse, tandis qu’avec un bêta baissier de 0.8, l’actif sera alors 20% moins réactif que le marché quand celui-ci baisse. </p>


#### 3. Drawdown Maximal
<p align="justify">
Le drawdown maximal est une mesure de risque, qui représente la plus forte baisse subie par un investissement sur une durée déterminée. En d'autres termes, c'est la perte maximale historique de la valeur d'un investissement par rapport à son plus haut historique. Afin de constituer ce plus haut historique, nous calculons la valeur cumulative maximale des rendements de la stratégie à chaque rendement individuel, que l’on soustrait des rendements de l’actif de la stratégie initiale. Nous obtenons alors les drawdowns, soit différentes baisses par rapport au plus haut historique. La plus grande perte par rapport au plus haut historique correspond à la valeur minimale parmi les drawdowns. C’est ainsi qu’on enregistre cette valeur en tant que drawdown maximal. </p>



#### 4. Kurtosis
<p align="justify">
Le kurtosis, ou coefficient d'aplatissement, évalue la forme de la distribution des rendements d'un actif financier. Une distribution normale a un kurtosis de 0, tandis qu'une distribution pointue (leptokurtique) a un kurtosis positif et une distribution aplatie (platikurtique) a un kurtosis négatif. Un kurtosis élevé indique plus de fluctuations par rapport aux rendements moyens. En général, une distribution aplatie est préférée car elle enregistre moins de performances extrêmes. Le kurtosis est donc une mesure de la fréquence à laquelle le prix d’un investissement peut fluctuer. Nous le calculons à partir du rendement de l’actif de notre stratégie de base en supprimant les valeurs manquantes. </p>


#### 5. Skewness
<p align="justify">
La Skewness ou le coefficient d’asymétrie, caractérise le degré d’asymétrie observé dans une distribution en probabilité, par rapport à sa moyenne. Une distribution suivant une loi normale est révélée par une skewness de 0. Une skewness négative indique une répartition des performances inférieure à la moyenne plus importante, soit une asymétrie vers la gauche du côté des valeurs les plus faibles. A contrario, une  skewness positive signifie une asymétrie vers la droite, avec les valeurs les plus élevées. Ainsi, un investissement à skewness positive est d’autant plus préférable. 
Ainsi, en accédant aux données des rendements de la stratégie non manquantes dans les rendements de la stratégie, et en utilisant la méthode .skew(), nous obtenons la skewness des données fournies. </p>


#### 6. Ratio de Sharpe 
<p align="justify">
Le ratio de Sharpe est une mesure de performance, évaluant le rendement ajusté en fonction du risque d'un portefeuille d'investissement. Sa formule consiste à diviser la surperformance du portefeuille en rapport avec le taux sans risque (comme l'EONIA) par le risque du portefeuille, qui est mesuré par l'écart-type de ses rendements. Un Ratio de Sharpe positif indique que le portefeuille a généré une surperformance par rapport au taux sans risque. Plus le ratio est élevé, meilleure sera la performance du portefeuille. Par exemple, un ratio de Sharpe de 0.4 signifie que le portefeuille a rapporté 0.4% de performance au-dessus du taux sans risque pour chaque unité de risque supplémentaire (mesurée par l'écart-type des rendements du portefeuille). Il convient de noter que le Ratio de Sharpe est généralement calculé sur des périodes supérieures à un an pour fournir une évaluation plus significative de la performance ajustée en fonction du risque.
Pour rendre le calcul plus simple, nous ne prenons pas en compte de taux sans risque. Le ratio de Sharpe est ainsi obtenu en divisant le rendement moyen annuel par la volatilité annuelle pour chaque actif financier. Ces valeurs, qui évaluent le rendement ajusté en fonction du risque, sont ensuite enregistrées dans le dictionnaire sharpe_ratio et sont ajoutées à une sous-liste. </p>


## IV. Visualiser les résultats de la stratégie : méthode **Plot**

<p align="justify">
Pour chaque symbole, nous traçons trois graphiques : un graphique de chandeliers japonais qui suit le cours du titre, un graphique pour visualiser les positions prises pour la stratégie, et un graphique pour l'évolution de la valeur du titre. </p>

<p align="justify">
Pour dessiner les chandeliers japonais, il nous faut d'abord convertir l'index du dataframe associé à chaque symbole en objet de date et d'heure. La commande mpf.plot() va alors tracer le graphique en prenant en paramètres le type de graphique souhaité, l'affichage du volume, le style du graphique et l'axe sur lequel tracer le graphique. </p>

<p align="justify">
On crée un deuxième sous-graphique pour tracer les positions prises pour chaque symbole. Les positions sont tracées en fonction de la date (index du dataframe). Les lignes  suivantes permettent de définir un titre, les étiquettes des axes et les emplacements des marques sur l'axe y. </p>

<p align="justify">
A l'aide de la fonction plt.plot() de matplotlib, nous traçons le graphique d'évolution de la performance pour chaque symbole désigné par la stratégie. Nous calculons la valeur de la stratégie en base 100 en ajoutant 1 au rendement de la stratégie pour chaque période ce qui nous permet de représenter la performance de la stratégie sous forme de valeur relative en partant d'une base de 100. Ensuite, plt.plot() nous permet de traçer l'évolution de la valeur de la stratégie au fil du temps. Pour une meilleure visibilité, nous ajoutons au graphique une légende pour indiquer la série de données représentée, un titre qui contient le symbole et des étiquettes d'axes. Enfin, nous affichons le graphique à l'écran. </p> 



## V. Tester le backtest 
<p align="justify">
Nous testons le code avec un exemple. La fonction mastrategie(data) implémente un exemple de stratégie de trading qui prend des positions basées sur la variation de prix entre les deux derniers jours. Tout d'abord, la liste positions est initialisée avec une seule valeur (0) pour la première journée, car il n'y a pas de position prise à ce moment-là. La boucle for itère sur les données à partir du deuxième jour jusqu'au dernier jour. Pour chaque jour, la variation de prix entre le jour actuel et le jour précédent est calculée à l'aide de la formule : (prix_jour_actuel - prix_jour_précédent) / prix_jour_précédent. En fonction de la variation de prix calculée, une position est prise :  <br> 
- Si la variation de prix est supérieure à 2%, une position d'achat est prise (représentée par 1).  <br>
- Si la variation de prix est inférieure à -2%, une position de vente est prise (représentée par -1).  <br>
- Sinon, aucune position n'est prise (représentée par 0).  <br>
La fonction renvoie uniquement la colonne "Position" du dataframe data dans laquelle on a stocké la liste des positions.  </p>


Résultats : 
 |                      |MSFT      |AAPL    |
 | :--------: |  :--------: | :--------: |
|Rendement moyen annuel | -0.152729 | -0.093623|
|Rendements cumulatifs  | -0.144101 | -0.095996|
|Volatilité quotidienne  | 0.005663 | 0.008006|
|Volatilité annualisée   | 0.089891 | 0.127093|
|Ratio de Sharpe        | -1.699048 | -0.736645|
|Bêta                    | 0.022524 | -0.163159|
|Bêta haussier          | -0.192035 | -0.262953|
|Bêta baissier           | 0.255934 | -0.403648|
|Drawndown maximal      | -0.048810 | -0.078070|
|Kurtosis               | 15.168969 | 12.109279|
|Skewness               | -0.048810 | -0.078070|

<p align="justify">
De ces résultats, nous pouvons dire que : <br>
- Rendement moyen annuel : La stratégie sur MSFT affiche un rendement moyen annuel légèrement plus bas que pour AAPL, ce qui indique une performance inférieure. <br>
- Rendements cumulatifs : Les rendements cumulatifs de la stratégie pour MSFT et AAPL sont tous deux négatifs, ce qui suggère une mauvaise performance de la stratégie testée. <br>
- Volatilité quotidienne et annualisée : La volatilité du rendements de la stratégie sur AAPL est plus élevée que celle pour MSFT, ce qui signifie que les rendements de la stratégie sur AAPL ont tendance à fluctuer davantage au jour le jour et sur une base annuelle. <br>
- Ratio de Sharpe : Le ratio de Sharpe de la stratégie est négatif pour les deux actions, ce qui indique un rendement ajusté au risque défavorable. Cependant, MSFT présente un ratio de Sharpe légèrement plus bas que AAPL, ce qui suggère une performance relative moins favorable en termes de rendement ajusté au risque. <br>
- Bêta : Les valeurs de bêta pour MSFT et AAPL sont proches de zéro, ce qui signifie que les rendements de la stratégie sont moins sensibles aux mouvements du marché global. <br>
- Drawdown maximal : la stratégie appliquée à MSFT affiche un drawdown maximal légèrement inférieur à celui de la stratégie appliquée à AAPL, ce qui indique une résilience relative supérieure lors de périodes de baisse du marché. <br>
- Kurtosis et Skewness : Les valeurs de kurtosis et de skewness pour les deux actions indiquent une distribution des rendements qui n'est pas parfaitement symétrique ni normale, ce qui suggère des caractéristiques de risque spécifiques à chaque stratégie. </p>

En résumé, on remarque que la stratégie backtestée n'est pas adéquate à ces deux titres. 

