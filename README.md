# MediSense

**Prédiction de la demande de médicaments et recommandation de réapprovisionnement par Machine Learning.**

Projet académique — Master Informatique et Télécommunications, Université Mohammed V de Rabat, Faculté des Sciences de Rabat (2025–2026).

Réalisé par **Jouiri Malak** et **Icharli Rania**, encadré par **Abdelhak Mahmoudi** (encadrant) et **Saad Frihi** (co-encadrant).

---

## Présentation

MediSense analyse cinq années d'historique de ventes pharmaceutiques (57 504 enregistrements, 30 médicaments, 13 catégories thérapeutiques) afin de :

- **Prédire** la demande journalière de chaque médicament sur un horizon ajustable, via 30 modèles Random Forest indépendants.
- **Recommander** des quantités de réapprovisionnement classées par niveau d'urgence (CRITIQUE, URGENT, MODÉRÉ, SUFFISANT).
- **Exposer** ces fonctionnalités via une API REST de 10 points d'accès.
- **Visualiser** les résultats dans un tableau de bord web interactif, incluant un assistant conversationnel (SenseBot).

**Performances obtenues :** R² moyen de 0,680, MAE moyen de 1,42, précision moyenne de 85,6 % sur les 30 modèles.

---

## Structure du dépôt

```
medisense/
├── generate_data.py          # Génération des 57 504 ventes synthétiques
├── train_model.py            # Entraînement des 30 modèles Random Forest
├── app.py                    # API REST Flask (10 endpoints)
├── pharmacy_dashboard.html   # Interface web (tableau de bord + chatbot)
├── requirements.txt          # Dépendances Python
├── data/
│   ├── sales_history.csv     # 57 504 lignes — ventes journalières
│   ├── stock_history.csv     # 1 890 lignes — stock mensuel
│   └── medications.csv       # 30 lignes — catalogue des médicaments
└── models/
    └── demand_predictor.pkl  # Généré par train_model.py (non versionné, voir .gitignore)
```

---

## Installation et lancement

```bash
# Cloner le dépôt
git clone <url-du-depot>
cd medisense

# Installer les dépendances
pip install -r requirements.txt

# Entraîner les 30 modèles (les données sont déjà fournies dans data/)
python train_model.py

# Démarrer l'API
python app.py
# → disponible sur http://localhost:5000
```

Ouvrir ensuite `pharmacy_dashboard.html` dans un navigateur.

**Connexion au tableau de bord :**

| Profil | Identifiant | Mot de passe |
|---|---|---|
| Administrateur | `admin` | `pharma2026` |
| Pharmacien | `pharmacien` | `pharma123` |
| Gestionnaire de stock | `gestionnaire` | `stock456` |

---

## API REST — endpoints principaux

| Méthode | Endpoint | Description |
|---|---|---|
| GET | `/api/medications` | Liste des 30 médicaments avec stock |
| GET | `/api/predict/:id?horizon=30` | Prédictions journalières |
| GET | `/api/recommend/:id` | Recommandation de réapprovisionnement |
| GET | `/api/recommend/all` | Toutes les recommandations triées par urgence |
| GET | `/api/model/metrics` | Métriques R², MAE, précision des modèles |
| POST | `/api/predict/custom` | Prédiction avec paramètres ajustables |

Documentation complète disponible sur la route `GET /`.

---

## Modèle Machine Learning

- **Algorithme :** Random Forest Regressor (scikit-learn)
- **Hyperparamètres :** 100 arbres, profondeur maximale 10, split train/test 80 % / 20 %
- **12 variables explicatives :** temporelles (année, mois, jour de semaine...), cycliques (encodage sinus/cosinus), mémoire (lags à 7/14/30 jours), tendance (moyennes mobiles)

---

## Licence

Projet académique réalisé dans le cadre du Master IT 2025-2026, Université Mohammed V de Rabat.
