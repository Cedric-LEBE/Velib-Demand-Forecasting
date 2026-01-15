# Projet Vélib - Prédiction de la demande de vélos

## 📋 Description du projet

Ce projet vise à développer un modèle prédictif pour anticiper la demande de vélos en libre-service (type Vélib). L'objectif est d'aider l'organisme de gestion à optimiser l'organisation de ses équipes de maintenance pour mieux répondre aux besoins des clients.

## 🎯 Objectif

Prédire le nombre de locations de vélos (`cnt`) en fonction de diverses variables météorologiques et temporelles afin d'optimiser la gestion des équipes de maintenance.

## 📊 Dataset

Le dataset contient 17 379 observations avec les variables suivantes :

### Variables explicatives
- **instant** : Index du relevé
- **dteday** : Date du relevé
- **season** : Saison (1=hiver, 2=printemps, 3=été, 4=automne)
- **mnth** : Mois (1-12)
- **hr** : Heure (0-23)
- **holiday** : Jour de vacances scolaires (booléen)
- **weekday** : Jour de la semaine (0-6)
- **weathersit** : Conditions météo (1=dégagé, 2=brouillard, 3=légère pluie/neige, 4=fortes averses)
- **temp** : Température en °C
- **atemp** : Température ressentie en °C
- **hum** : Taux d'humidité
- **windspeed** : Vitesse du vent

### Variable cible
- **count** – nombre total de locations de vélos 

## 🛠️ Technologies utilisées

- **Python 3.8+**
- **pandas** : Manipulation et analyse de données
- **numpy** : Calculs numériques
- **matplotlib** & **seaborn** : Visualisation de données
- **scikit-learn** : Machine Learning
  - Random Forest Regressor
  - K-Means Clustering
  - GridSearchCV pour l'optimisation des hyperparamètres

## 📁 Structure du projet

```
Velib-Demand-Forecasting/
├── data/
│   └── input/
│       └── velo.csv           # Dataset principal
├── training.ipynb             # Notebook d'analyse et modélisation
├── pyproject.toml             # Configuration du projet
└── README.md                  # Ce fichier
```

## 🚀 Installation

### Prérequis
- Python 3.8 ou supérieur
- pip ou conda

### Étape 1 : Créer un environnement virtuel

```bash
# Créer un environnement virtuel
python3 -m venv .venv

# Activer l'environnement virtuel
# Sur macOS/Linux :
source .venv/bin/activate

# Sur Windows :
venv\Scripts\activate
```

### Étape 2 : Installation des dépendances

```bash
# Avec pip
pip install -e .

# Ou installer les dépendances directement
pip install pandas numpy matplotlib seaborn scikit-learn jupyter ipykernel
```

### Étape 3 : Configuration du kernel Jupyter

Pour utiliser l'environnement virtuel dans Jupyter, il faut l'enregistrer comme kernel :

```bash
# S'assurer que l'environnement virtuel est activé
python -m ipykernel install --user --name=velib-env --display-name="Python (Velib)"
```

Ensuite, lors de l'ouverture du notebook dans Jupyter ou VS Code :
1. Ouvrir `training.ipynb`
2. Sélectionner le kernel "Python (Velib)" dans le menu de sélection du kernel
3. Le notebook utilisera désormais l'environnement virtuel avec toutes les dépendances installées

## 📈 Méthodologie

### 1. Exploration des données
- Analyse de la qualité des données (valeurs manquantes, aberrantes)
- Visualisation des distributions et corrélations
- Identification des patterns temporels et météorologiques

### 2. Feature Engineering
Création de nouvelles variables :
- `is_weekend` : Indicateur weekend/semaine
- `is_rush_hour` : Indicateur heures de pointe (7-9h et 17-19h)
- `temp_humidity_interaction` : Interaction température × humidité

### 3. Approche non-supervisée
- K-Means clustering pour identifier des profils de demande similaires
- Analyse via PCA (Principal Component Analysis)

### 4. Modélisation prédictive
- **Algorithme** : Random Forest Regressor
- **Pipeline** : Preprocessing automatique (OneHotEncoding pour variables catégorielles)
- **Validation** : Split train/test (80/20)
- **Optimisation** : GridSearchCV avec validation croisée (5-fold)

### 5. Métriques d'évaluation
- **RMSE** (Root Mean Squared Error) : Erreur quadratique moyenne
- **MAE** (Mean Absolute Error) : Erreur absolue moyenne
- **R²** : Coefficient de détermination

## 📊 Résultats

### Observations clés
- Forte corrélation entre la demande et :
  - La température (r=0.40)
  - Les heures de la journée (r=0.39)
  - L'humidité (r=-0.32)
- Impact significatif des saisons et conditions météorologiques
- Patterns distincts entre jours de semaine et weekends
- Pics de demande aux heures de pointe

### Performance du modèle
Le modèle Random Forest optimisé offre une bonne capacité prédictive avec un R² élevé et un faible RMSE, permettant une planification efficace des équipes de maintenance.

## 🔍 Utilisation

1. Ouvrir le notebook Jupyter :
```bash
jupyter notebook training.ipynb
```

2. Exécuter les cellules séquentiellement pour :
   - Charger et explorer les données
   - Effectuer le feature engineering
   - Entraîner les modèles
   - Évaluer les performances

## 📝 Points d'attention

- **Valeurs manquantes** : 1 664 valeurs manquantes pour l'humidité (~9.6%)
- **Multicolinéarité** : Variables `casual` et `registered` exclues du modèle (composantes de `cnt`)
- **Régularisation** : Paramètres ajustés pour éviter le sur-apprentissage

