# 🏦 Modélisation du Risque de Crédit IFRS 9 – Compétition Kaggle Home Credit

Ce projet est basé sur la compétition [Home Credit Default Risk](https://www.kaggle.com/competitions/home-credit-default-risk/overview) organisée sur Kaggle. L’objectif est d’anticiper les défauts de paiement de clients à partir de leurs données socio-économiques et comportementales, et d’adapter ce cadre au calcul réglementaire de la perte attendue selon la norme **IFRS 9**.

---

## 🎯 Objectifs du projet

- Construire un pipeline complet de **modélisation du risque de crédit** :
  - Estimation de la **probabilité de défaut (PD)**.
  - Modélisation de la **perte en cas de défaut (LGD)**.
  - Simulation de l’**exposition au moment du défaut (EAD)**.
  
- Appliquer ces estimations pour évaluer le risque global du portefeuille à travers :

  - **EAD** : montant restant dû par les clients au moment de leur défaut.
  - **EAC** *(Expected Accrued Cost)* : perte attendue mensuelle sur les flux de remboursement.  
    $\text{EAC}_t = \sum_{i \in \text{actifs}} PD_i(t) \cdot LGD_i \cdot \text{mensualité}_i$
  - **ECL** *(Expected Credit Loss)* : perte attendue sur l’encours restant si le client fait défaut à l’instant \( t \).
    $\text{ECL}_t = \sum_{i \in \text{actifs}} PD_i(t) \cdot LGD_i \cdot EAD_i(t)$


- Explorer la **dynamique du risque de crédit** via des simulations Monte Carlo.

---

## 🗂️ Contenu du projet

| Notebook                        | Description |
|---------------------------------|-------------|
| `01_data_and_features.ipynb`    | Nettoyage des données Kaggle, ingénierie des variables (features), encodage, scaling, etc. |
| `02_modelisation_pd.ipynb`      | Modélisation de la **PD** via des modèles de classification (Logistic Regression, Random Forest, LightGBM). |
| `03_modelisation_lgd.ipynb`     | Estimation de la **LGD** via des modèles de régression, incluant interprétabilité avec SHAP. |
| `04_simulation_ead.ipynb`       | Simulation de l’évolution de l’encours et estimation de l’**EAD au moment du défaut** avec dynamique mensuelle. |

---

## ⚙️ Méthodologie

- **Prétraitement** : Fusion des jeux de données Kaggle, nettoyage, gestion des valeurs manquantes.
- **Feature Engineering** : Création de variables agrégées (revenu, historique de crédit, dépenses, ratio dette/revenu…).
- **Modélisation** :
  - **Classification binaire** pour la PD.
  - **Régression** pour la LGD.
  - **Simulation** du comportement du crédit dans le temps pour l’EAD.
- **Validation croisée** adaptée aux séries temporelles (TimeSeriesSplit ou GroupKFold).
- **Interprétabilité** des modèles via SHAP pour la LGD et importance des features pour la PD.

---

## 📈 Résultats attendus

- Courbes ROC, matrices de confusion et scores (AUC, F1, RMSE).
- Graphe de la distribution des pertes agrégées simulées.
- Tableaux synthétiques par décile de risque.

---

## 📈 Résultats – Simulation du risque de crédit

La simulation Monte Carlo sur 100 trajectoires permet de visualiser la dynamique du portefeuille de crédits dans le temps.

![Simulation EAD / EAC / ECL](simulation_risque_credit.png)

🔍 **Lecture des courbes :**
- **EAD (bleu)** : exposition au moment du défaut – élevée au départ, elle décroît fortement avec l’extinction progressive du portefeuille.
- **EAC (rouge)** : coût attendu sur les flux mensuels – reflète le risque sur les remboursements à venir, concentré en début de période.
- **ECL (vert)** : perte attendue sur encours – pondère l’exposition restante par le risque de défaut.

📌 Ces résultats illustrent l’importance du provisionnement initial IFRS 9, car **le risque est massivement porté au début de vie des crédits**, tant en termes d’exposition que de pertes potentielles.

---

## 📌 Avertissement

> Ce projet est à but **éducatif** et repose sur des données publiques. Il ne constitue pas un dispositif réglementaire conforme en production bancaire, mais une **preuve de concept pédagogique**.

---

## 🔗 Références

- 🔍 [Kaggle - Home Credit Default Risk](https://www.kaggle.com/competitions/home-credit-default-risk/overview)
- 📄 Norme IFRS 9 – Financial Instruments
