# Prévision des ventes hebdomadaires de Walmart

## Présentation
Ce projet propose une modélisation supervisée des ventes hebdomadaires de magasins Walmart à partir de variables magasin, calendaires et macroéconomiques. Il s'inscrit dans un cadre pédagogique centré sur la régression, avec une démarche volontairement simple, lisible et reproductible.

L'objectif n'est pas de construire le modèle le plus complexe possible, mais de mettre en place une chaîne de travail cohérente : compréhension des données, préparation, modélisation, comparaison des résultats et interprétation.

## Objectif
Prédire la variable cible `Weekly_Sales` et comparer plusieurs modèles de régression afin d'identifier une solution :
- interprétable ;
- cohérente avec le brief ;
- techniquement défendable ;
- adaptée à un exercice de modélisation supervisée.

## Contexte métier
Dans un contexte retail, une estimation des ventes hebdomadaires peut contribuer à :
- mieux anticiper l'activité de certains magasins ;
- apprécier l'effet du calendrier et des périodes de fête ;
- mettre en perspective quelques variables de contexte économique ;
- appuyer des décisions opérationnelles ou marketing simples.

Ce travail reste toutefois un projet pédagogique. Les résultats doivent donc être lus comme une démonstration méthodologique, et non comme un outil directement déployable en production.

## Données
Le jeu de données utilisé est disponible dans le dossier `data/` :

- `data/Walmart_Store_sales.csv`

Variables principales :
- `Store`
- `Date`
- `Holiday_Flag`
- `Temperature`
- `Fuel_Price`
- `CPI`
- `Unemployment`
- `Weekly_Sales` *(variable cible)*

## Démarche
Le projet suit une progression continue, de la compréhension des données jusqu'à la comparaison des modèles.

### 1. Analyse exploratoire
L'analyse exploratoire permet de :
- comprendre la structure du dataset ;
- repérer les valeurs manquantes ;
- observer la distribution des ventes ;
- identifier les variables potentiellement les plus informatives.

### 2. Préparation des données
Les principales opérations de préparation sont les suivantes :
- suppression des lignes où `Weekly_Sales` est manquant ;
- conversion de `Date` ;
- création de variables calendaires (`Year`, `Month`, `Day`, `DayOfWeek`) ;
- retrait des valeurs extrêmes selon la règle des 3 sigmas sur certaines variables numériques ;
- séparation des variables numériques et catégorielles.

### 3. Prétraitement avec `scikit-learn`
Le pipeline de préparation applique :
- une imputation des valeurs manquantes ;
- un encodage des variables catégorielles ;
- une standardisation des variables numériques.

### 4. Modèles comparés
Trois modèles de régression sont entraînés et comparés :
- `LinearRegression`
- `Ridge`
- `Lasso`

Les modèles régularisés sont optimisés avec `GridSearchCV`.

## Résultats principaux
Les performances observées sur le jeu de test sont résumées ci-dessous :

| Modèle | R² test | RMSE test |
|---|---:|---:|
| LinearRegression | 0,898 | 188 478 |
| Ridge | 0,899 | 187 078 |
| Lasso | 0,901 | 185 280 |

Dans ce cadre, **Lasso** obtient le meilleur compromis sur le critère retenu ici, le **RMSE sur le jeu de test**.

## Enseignements principaux
L'analyse fait ressortir plusieurs points utiles :
- l'identifiant du magasin porte une part importante du signal ;
- la dimension calendaire reste informative, notamment via le mois ;
- les variables macroéconomiques contribuent à la prédiction, sans expliquer à elles seules l'essentiel des ventes ;
- `DayOfWeek` apporte peu d'information ici, ce qui reste cohérent avec la structure effective des dates disponibles.

## Limites
Quelques limites doivent être gardées en tête :
- le dataset reste de petite taille ;
- certaines situations, comme les semaines de fête, sont peu représentées ;
- la modélisation demeure linéaire ;
- les écarts entre Ridge et Lasso sont réels, mais modestes.

## Structure du dépôt
```text
.
├── .gitignore
├── README.md
├── requirements.txt
├── walmart_weekly_sales_clean.ipynb
├── walmart_weekly_sales_executed.ipynb
└── data/
    └── Walmart_Store_sales.csv
```

## Utilisation
### Consulter le projet
Ouvrir directement `walmart_weekly_sales_executed.ipynb` pour visualiser le notebook avec les tableaux et graphiques déjà générés.

### Réexécuter le projet
1. Installer Python 3.10+ ou 3.11.
2. Installer les dépendances :
   ```bash
   pip install -r requirements.txt
   ```
3. Vérifier que le fichier `data/Walmart_Store_sales.csv` est bien présent.
4. Ouvrir `walmart_weekly_sales_clean.ipynb`.
5. Exécuter les cellules dans l'ordre.

## Technologies utilisées
- Python
- pandas
- NumPy
- Matplotlib
- scikit-learn
- Jupyter Notebook

## Contenu du dépôt
- `walmart_weekly_sales_executed.ipynb` : version exécutée, avec sorties visibles ;
- `walmart_weekly_sales_clean.ipynb` : version légère, sans sorties ;
- `data/Walmart_Store_sales.csv` : jeu de données utilisé ;
- `requirements.txt` : dépendances Python minimales ;
- `README.md` : présentation du projet.

## Conclusion
Ce projet montre qu'une démarche de modélisation sobre, structurée et alignée avec le brief permet déjà d'obtenir une prédiction convaincante des ventes hebdomadaires. L'intérêt principal du travail réside dans la cohérence de la méthode, la lisibilité du pipeline et la comparaison raisonnée entre modèle de base et modèles régularisés.
