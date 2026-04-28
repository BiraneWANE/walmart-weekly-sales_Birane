# Prédiction des ventes hebdomadaires de Walmart

## Présentation du projet
Ce projet a pour objectif de prédire les **ventes hebdomadaires** de magasins Walmart à partir d'informations magasin, calendaires et macroéconomiques.

Le travail est volontairement aligné avec le cadre pédagogique du module :
- analyse exploratoire des données ;
- preprocessing avec `pandas` puis `scikit-learn` ;
- modèle de base avec **régression linéaire** ;
- réduction de l'overfitting avec **Ridge** et **Lasso** ;
- comparaison des performances et interprétation des coefficients.

## Contexte métier
Walmart souhaite mieux comprendre les facteurs associés au niveau de ventes hebdomadaires de ses magasins. Un modèle de prévision, même simple, peut aider à :
- mieux lire les effets du calendrier et du contexte économique ;
- comparer les magasins ;
- éclairer certaines décisions marketing ou opérationnelles ;
- disposer d'une estimation rapide des ventes à partir des variables disponibles.

## Objectif
Construire un modèle de régression capable de prédire `Weekly_Sales` avec une bonne précision, tout en restant :
- lisible ;
- reproductible ;
- interprétable ;
- cohérent avec le brief et le cours.

## Données utilisées
Le fichier principal est :
- `Walmart_Store_sales.csv`

Variables principales utilisées :
- `Store`
- `Holiday_Flag`
- `Temperature`
- `Fuel_Price`
- `CPI`
- `Unemployment`
- `Date`
- `Weekly_Sales` (cible)

## Méthodologie
### 1. Audit initial
Le dataset brut contient **150 lignes**. La cible `Weekly_Sales` présente des valeurs manquantes, qui sont supprimées conformément au brief.

### 2. Préparation des données
Les étapes de préparation respectent le cadre demandé :
- suppression des lignes où `Weekly_Sales` est manquant ;
- conversion de `Date` et création de `Year`, `Month`, `Day`, `DayOfWeek` ;
- retrait des valeurs extrêmes selon la règle des **3 sigmas** sur `Temperature`, `Fuel_Price`, `CPI` et `Unemployment` ;
- séparation entre variables catégorielles et numériques.

Après nettoyage, le dataset de travail contient **131 lignes**.

### 3. Analyse exploratoire
L'EDA met en évidence plusieurs points utiles :
- le **magasin** est un signal très structurant ;
- les semaines marquées par `Holiday_Flag` ont en moyenne des ventes plus élevées, avec prudence car elles sont peu nombreuses ;
- `Month` semble capter un signal calendaire utile ;
- certaines variables macroéconomiques, notamment `CPI` et `Unemployment`, montrent une relation avec la cible.

### 4. Pipeline de preprocessing
Le pipeline `scikit-learn` applique :
- une imputation des valeurs manquantes ;
- un **one-hot encoding** sur `Store` et `Holiday_Flag` ;
- une standardisation des variables numériques.

### 5. Modèles entraînés
Trois modèles sont comparés :
- `LinearRegression`
- `Ridge`
- `Lasso`

Les modèles régularisés sont optimisés avec `GridSearchCV`.

## Résultats principaux
### Performances sur le jeu de test
- **LinearRegression** : R² ≈ **0,898** ; RMSE ≈ **188 478**
- **Ridge** : R² ≈ **0,899** ; RMSE ≈ **187 078**
- **Lasso** : R² ≈ **0,901** ; RMSE ≈ **185 280**

### Modèle retenu
Le modèle retenu est **Lasso**, car il obtient le meilleur compromis sur le critère principal retenu ici, le **RMSE test**.

## Principaux enseignements
- l'identité du **magasin** explique une large part de la variance des ventes ;
- l'information **calendaire** reste utile, notamment `Month` ;
- les variables macroéconomiques contribuent à la prédiction, sans constituer à elles seules l'essentiel du signal ;
- `DayOfWeek` est peu utile dans ce dataset, ce qui est cohérent avec le fait que les dates valides tombent presque toutes le même jour de la semaine.

## Limites
- le dataset est **petit**, ce qui limite la robustesse des conclusions ;
- les semaines de vacances ou de fête sont peu nombreuses ;
- le modèle reste **linéaire** et ne capture pas d'éventuels effets non linéaires ;
- le meilleur RMSE est obtenu par Lasso, mais Ridge reste très proche, ce qui rappelle que les écarts sont ici modestes.

## Structure du dépôt
```text
.
├── README.md
├── .gitignore
├── Walmart_Store_sales.csv
├── walmart_weekly_sales_executed.ipynb
├── walmart_weekly_sales_clean.ipynb
├── audit_projet.md
├── plan_publication_github.md
├── checklist_avant_push.md
└── guide_git.md
```

## Exécution
### Option 1 — lecture simple
Ouvrir directement :
- `walmart_weekly_sales_executed.ipynb`

### Option 2 — réexécuter le projet
1. Installer Python 3.10+ ou 3.11.
2. Installer les dépendances principales :
   ```bash
   pip install pandas numpy matplotlib scikit-learn jupyter ipython
   ```
3. Placer `Walmart_Store_sales.csv` à la racine du projet.
4. Ouvrir puis exécuter `walmart_weekly_sales_clean.ipynb`.

## Fichiers recommandés pour GitHub
À publier :
- `README.md`
- `.gitignore`
- `walmart_weekly_sales_executed.ipynb`
- `walmart_weekly_sales_clean.ipynb`
- `Walmart_Store_sales.csv` si la diffusion du dataset est autorisée
- `audit_projet.md`
- `plan_publication_github.md`
- `checklist_avant_push.md`
- `guide_git.md`

À ne pas publier :
- archives `.zip`
- fichiers de cours complets
- anciennes versions du notebook
- exports HTML non nécessaires
- fichiers temporaires et caches

## Conclusion
Le projet répond au brief avec une démarche simple, propre et cohérente : une baseline linéaire est construite, puis comparée à des versions régularisées. Le résultat final montre qu'un modèle **interprétable** peut déjà fournir une estimation convaincante des ventes hebdomadaires dans ce cadre.
