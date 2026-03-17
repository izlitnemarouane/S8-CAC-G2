# Compte Rendu de Projet — Intelligence Artificielle
## Audit Financier & Détection d'Anomalies : Classification des Risques d'Audit par Cycle Comptable

---

| Champ | Détail |
|-------|--------|
| **Étudiants** | IZLITNE Marouane & NOUBAIR Ziad |
| **Filière** | CAC 2 (Commissariat aux Comptes — 2ème année) |
| **Module** | Intelligence Artificielle appliquée à l'Audit |
| **Année académique** | 2025–2026 |
| **Référentiel normatif** | Normes ISA (IFAC) + Guides OECCA Maroc |
| **Outil principal** | Python 3.10 — Google Colab |
| **Fichier notebook** | `Projet_IA_CAC2_IZLITNE_MAROUANE_ET_NOUBAIR_ZIAD.ipynb` |

---

## Table des matières

1. [Contexte et problématique](#1-contexte-et-problématique)
2. [Objectifs du projet](#2-objectifs-du-projet)
3. [Description des données](#3-description-des-données)
4. [Feature Engineering](#4-feature-engineering)
5. [Analyse exploratoire des données (EDA)](#5-analyse-exploratoire-des-données-eda)
6. [Préparation des données pour la modélisation](#6-préparation-des-données-pour-la-modélisation)
7. [Modèles testés](#7-modèles-testés)
8. [Résultats et évaluation](#8-résultats-et-évaluation)
9. [Cartographie des risques et planification des missions](#9-cartographie-des-risques-et-planification-des-missions)
10. [Discussion critique](#10-discussion-critique)
11. [Conclusion](#11-conclusion)
12. [Références normatives](#12-références-normatives)

---

## 1. Contexte et problématique

### 1.1 Contexte général

La profession d'auditeur financier est au cœur d'une transformation numérique profonde. L'émergence de l'intelligence artificielle offre désormais la possibilité d'automatiser et de renforcer des processus jusqu'ici fondés exclusivement sur le jugement professionnel humain. Parmi ces processus, l'**évaluation et la classification des risques d'audit** constitue l'une des étapes les plus stratégiques de toute mission, conformément aux exigences de la norme ISA 315 (*Identification et évaluation des risques d'anomalies significatives*).

Au Maroc, le cadre normatif de l'audit est encadré par l'**OECCA** (Ordre des Experts Comptables et Comptables Agréés) qui a adopté les normes ISA dans leur version traduite et adaptée au contexte national, notamment via le référentiel du **Plan Comptable Général Marocain (PCG Maroc)**. La mise en œuvre de ces normes implique pour chaque mission d'audit :

- l'identification systématique des risques par cycle comptable (ventes, achats, trésorerie, paie, etc.) ;
- leur classification selon un niveau de criticité (faible, moyen, élevé) ;
- la conception de procédures d'audit adaptées au niveau de risque détecté.

### 1.2 Problématique

La réalisation manuelle de cette classification est chronophage, subjective et sujette à des biais humains. La question centrale de ce projet est la suivante :

> **Dans quelle mesure un modèle de machine learning peut-il automatiser la classification des risques d'audit par cycle comptable, en s'appuyant sur des données simulées conformes aux normes ISA et aux pratiques de l'OECCA Maroc ?**

Cette question implique des sous-problèmes techniques et méthodologiques : la construction de features pertinentes à partir de données qualitatives, le choix et la comparaison de plusieurs algorithmes de classification, et l'interprétation des résultats dans une perspective opérationnelle d'audit.

---

## 2. Objectifs du projet

Le projet poursuit quatre objectifs principaux, articulés autour des attentes pédagogiques du module IA :

| # | Objectif | Lien avec l'audit |
|---|----------|-------------------|
| 1 | Construire une base de données simulée de risques d'audit par cycle comptable | ISA 315 — identification des risques |
| 2 | Réaliser un feature engineering pertinent sur des indicateurs financiers et comptables | ISA 315.25 — évaluation ordinale |
| 3 | Comparer plusieurs algorithmes de classification supervisée | ISA 330 — adaptation des procédures |
| 4 | Produire un programme de travail d'audit priorisé et exportable | ISA 300 — planification de la mission |

---

## 3. Description des données

### 3.1 Source et nature

Le dataset utilisé est un **jeu de données simulé**, construit sur la base des référentiels ISA et des pratiques documentaires de l'OECCA Maroc. Il est stocké dans un fichier Excel (`base_donnees_risques_audit_cycle_comptable.xlsx`), feuille `Audit_Risks_DB`, et chargé via la bibliothèque `pandas` depuis Google Drive.

> Le recours à des données simulées est justifié par la confidentialité inhérente aux dossiers d'audit réels et par la nécessité de disposer d'un dataset équilibré et pédagogiquement représentatif.

### 3.2 Structure du dataset

Le dataset comprend **220 observations** (risques d'audit identifiés) et **10 variables** :

| Variable | Type | Description |
|----------|------|-------------|
| `ID` | Texte | Identifiant unique du risque (R001 → R220) |
| `Cycle comptable` | Catégorielle | Cycle d'appartenance du risque (8 cycles) |
| `Processus` | Catégorielle | Sous-processus comptable concerné |
| `Compte concerné (PCG Maroc)` | Texte | Numéro(s) de compte selon le PCG Maroc |
| `Description du risque` | Texte | Libellé descriptif du risque identifié |
| `Type de risque` | Catégorielle | Nature du risque : Inhérent / Contrôle / Fraude |
| `Assertion impactée` | Catégorielle | Assertion d'audit concernée (6 modalités) |
| `Niveau de risque` | Catégorielle | **Variable cible** : Faible / Moyen / Élevé |
| `Procédure d'audit` | Texte | Type de procédure à mettre en œuvre |
| `Documents de preuve` | Texte | Pièces justificatives attendues |

### 3.3 Cycles comptables couverts

Le dataset couvre **8 cycles comptables**, représentant l'ensemble des grands domaines d'un audit d'états financiers :

| Cycle comptable | Nb de risques | Part (%) |
|-----------------|:-------------:|:--------:|
| Ventes - Clients | 30 | 13,6% |
| Achats - Fournisseurs | 30 | 13,6% |
| Trésorerie | 30 | 13,6% |
| Stocks | 30 | 13,6% |
| Immobilisations | 30 | 13,6% |
| Paie | 30 | 13,6% |
| Fiscalité | 30 | 13,6% |
| Capitaux propres | 10 | 4,5% |
| **Total** | **220** | **100%** |

### 3.4 Distribution de la variable cible

La variable cible `Niveau de risque` présente une distribution **parfaitement équilibrée** entre les trois classes :

| Niveau de risque | Occurrences | Proportion |
|:----------------:|:-----------:|:----------:|
| 🟢 Faible | 73 | 33,2% |
| 🟡 Moyen | 74 | 33,6% |
| 🔴 Élevé | 73 | 33,2% |

Cet équilibre est un atout majeur pour la modélisation : il évite les biais de classe dominante et rend inutile le recours à des techniques de rééchantillonnage (SMOTE, undersampling, etc.).

### 3.5 Qualité des données

L'exploration initiale révèle une **absence totale de valeurs manquantes** sur l'ensemble des colonnes, ce qui confirme la fiabilité du processus de simulation. Toutes les variables catégorielles sont cohérentes et sans anomalie de format.

---

## 4. Feature Engineering

Le feature engineering est l'étape la plus créative et la plus critique du projet. Elle consiste à transformer les données brutes en indicateurs numériques exploitables par les algorithmes de classification. Chaque feature construite est justifiée par une référence normative ISA.

### 4.1 Features construites

#### Feature 1 — Encodage ordinal du niveau de risque (`risque_score`)
**Justification ISA 315.25** : l'évaluation du risque repose sur une échelle ordinale.

```
Faible → 0 | Moyen → 1 | Élevé → 2
```

#### Feature 2 — Score de criticité par type de risque (`type_risque_score`)
**Justification ISA 200 & 240** : les risques de fraude présentent une intentionnalité qui justifie un coefficient plus élevé.

```
Inhérent → 1 | Contrôle → 2 | Fraude → 3
```

#### Feature 3 — Score d'assertion (`assertion_score`)
**Justification ISA 315** : les assertions d'Existence et d'Exhaustivité sont au cœur des manipulations comptables les plus fréquentes.

```
Autorisation, Présentation → 1
Évaluation, Cut-off        → 2
Existence, Exhaustivité    → 3
```

#### Feature 4 — Indice de Risque Combiné (`IRC`)
Indicateur synthétique inspiré du modèle de risque d'audit (Risque d'audit = Risque inhérent × Risque de contrôle × Risque de détection), normalisé entre 0 et 1 :

```
IRC = (type_risque_score × assertion_score) / (3 × 3)
```

Statistiques obtenues : moyenne = 0,557 · écart-type = 0,270 · min = 0,111 · max = 1,000

#### Feature 5 — Complexité du cycle (`complexite_cycle`)
Nombre de processus distincts par cycle. Un cycle plus complexe implique davantage de diligences.

#### Feature 6 — Nombre de comptes PCG concernés (`nb_comptes_pcg`)
Un risque touchant plusieurs comptes du PCG Maroc est plus difficile à circonscrire lors de l'audit.

#### Feature 7 — Encodage one-hot des variables catégorielles
Les variables `Cycle comptable`, `Type de risque` et `Assertion impactée` sont transformées en vecteurs binaires via `pd.get_dummies()`, produisant **17 features supplémentaires**.

#### Feature 8 — Score de priorité d'audit (`priorite_audit`)
Score composite pondéré servant à la planification opérationnelle de la mission (ISA 300) :

```
priorite_audit = type_risque_score × 0,3 + assertion_score × 0,2 + risque_score × 0,5
```

### 4.2 Matrice finale de features

La matrice `X` finale comprend **23 features** (6 numériques + 17 one-hot), pour 220 observations.

---

## 5. Analyse exploratoire des données (EDA)

L'EDA a mobilisé `matplotlib` et `seaborn` pour produire 11 visualisations structurées en 5 axes d'analyse.

### 5.1 Distribution globale des niveaux de risque

Les graphiques en barres et en camembert confirment la répartition équilibrée (~33% par classe). L'équilibre est intentionnel dans la simulation et facilite l'apprentissage des modèles.

### 5.2 Répartition par cycle comptable

Chaque cycle présente exactement 10 risques par niveau (sauf Capitaux propres avec ~3-4 par niveau), reflétant une couverture homogène de l'univers d'audit. Aucun cycle ne se démarque par une surconcentration de risques élevés, ce qui est cohérent avec la nature simulée du dataset.

### 5.3 Heatmap Cycle × Assertion (risques élevés)

La heatmap des risques élevés croisée par cycle et assertion révèle que les assertions d'**Existence** et d'**Évaluation** concentrent le plus de risques élevés, en particulier dans les cycles Ventes-Clients et Achats-Fournisseurs. Ces zones correspondent aux zones rouges prioritaires de l'audit ISA 315.

### 5.4 Types de risques par cycle

La représentation en barres empilées horizontales montre une répartition quasi-équitable des trois types de risques (Inhérent, Contrôle, Fraude) dans chaque cycle. Le cycle **Fiscalité** présente une proportion légèrement plus élevée de risques inhérents, cohérente avec la complexité réglementaire de la matière fiscale au Maroc.

### 5.5 Distribution de l'Indice de Risque Combiné (IRC)

L'histogramme de l'IRC par niveau montre que les trois classes se distinguent clairement sur l'axe de l'IRC :
- Risques Faibles : IRC concentré entre 0,1 et 0,4
- Risques Moyens : IRC réparti entre 0,3 et 0,7
- Risques Élevés : IRC concentré entre 0,5 et 1,0

Cette séparabilité explique en grande partie les excellentes performances obtenues lors de la modélisation.

---

## 6. Préparation des données pour la modélisation

### 6.1 Encodage de la variable cible

La variable cible est encodée via `LabelEncoder` :

```
Faible → 0 | Moyen → 1 | Élevé → 2
```

### 6.2 Division du dataset

La division entraînement/test est réalisée avec un ratio **80% / 20%** et une **stratification** sur la variable cible, garantissant une représentation équilibrée des classes dans les deux sous-ensembles.

| Ensemble | Taille | Faible | Moyen | Élevé |
|----------|:------:|:------:|:-----:|:-----:|
| Entraînement | 176 (80%) | 58 | 59 | 59 |
| Test | 44 (20%) | 15 | 15 | 14 |

### 6.3 Normalisation

Pour les modèles sensibles à l'échelle des variables (Régression Logistique et KNN), une normalisation `StandardScaler` est appliquée sur les features numériques (µ=0, σ=1). Les modèles à base d'arbres (Decision Tree, Random Forest, Gradient Boosting) utilisent les données brutes non normalisées.

---

## 7. Modèles testés

Cinq algorithmes de classification supervisée ont été sélectionnés pour couvrir des familles méthodologiques différentes :

### 7.1 Présentation des modèles

| Modèle | Famille | Paramètres clés |
|--------|---------|-----------------|
| **Régression Logistique** | Modèle linéaire | `max_iter=500`, `class_weight='balanced'` |
| **Arbre de Décision (CART)** | Arbre | `max_depth=6`, `class_weight='balanced'` |
| **Random Forest** | Ensemble (bagging) | `n_estimators=200`, `max_depth=8`, `class_weight='balanced'` |
| **Gradient Boosting** | Ensemble (boosting) | `n_estimators=150`, `learning_rate=0.1`, `max_depth=4` |
| **K-Nearest Neighbors** | Instance-based | `n_neighbors=7`, `metric='euclidean'` |

### 7.2 Protocole d'évaluation

L'évaluation est conduite sur deux niveaux :
- **Test set** (44 observations) : accuracy, F1-Score macro
- **Validation croisée stratifiée** (StratifiedKFold, k=5, sur le train set) : accuracy moyenne et écart-type

---

## 8. Résultats et évaluation

### 8.1 Tableau comparatif des performances

| Modèle | Accuracy Test | F1-Score Macro | CV Mean (5-fold) | CV Std |
|--------|:-------------:|:--------------:|:----------------:|:------:|
| **Régression Logistique** | **1,000** | **1,000** | **1,000** | **0,000** |
| Arbre de Décision | 1,000 | 1,000 | 1,000 | 0,000 |
| Random Forest | 1,000 | 1,000 | 1,000 | 0,000 |
| Gradient Boosting | 1,000 | 1,000 | 1,000 | 0,000 |
| K-Nearest Neighbors | 0,955 | 0,954 | 0,915 | 0,018 |

### 8.2 Analyse des résultats

**Quatre modèles sur cinq atteignent une performance parfaite** (accuracy = 1,000, F1-macro = 1,000 sur test et validation croisée). Le seul modèle présentant des imperfections est le KNN (accuracy = 95,5%), ce qui est cohérent avec ses limitations connues : sensibilité à la dimensionnalité de l'espace des features (23 dimensions) et absence de mécanisme d'apprentissage de structure latente.

**Interprétation des scores parfaits :**

Les performances de 100% s'expliquent par la nature déterministe des données simulées. Le dataset ayant été construit par règles explicites (chaque combinaison cycle/type/assertion détermine mécaniquement le niveau de risque), les modèles apprennent des frontières de décision exactes. Cela ne constitue pas un problème de surapprentissage (*overfitting*) mais reflète la structure intrinsèque du dataset simulé. Dans un contexte de données réelles, ces scores baisseraient inévitablement en raison de la variabilité et de l'ambiguïté propres aux situations d'audit terrain.

**Le modèle retenu** comme référence est la **Régression Logistique**, sélectionnée à iso-performance pour sa meilleure **interprétabilité** (coefficients lisibles) et sa conformité avec la logique de transparence requise dans les missions d'audit professionnelles.

### 8.3 Matrice de confusion

La matrice de confusion pour le meilleur modèle (Régression Logistique) présente une diagonale parfaite :

```
           Prédit
           Faible  Moyen  Élevé
Réel Faible   15      0      0
     Moyen     0     15      0
     Élevé     0      0     14
```

Aucune erreur de classification : les 44 observations du test set sont toutes correctement classées.

### 8.4 Importance des features (Random Forest)

L'analyse de l'importance des features via le critère Gini du Random Forest révèle la hiérarchie suivante des variables déterminantes :

1. `type_risque_score` — le type de risque (Inhérent/Contrôle/Fraude) est le prédicteur dominant
2. `IRC` — l'indice de risque combiné, feature synthétique construite
3. `assertion_score` — la criticité de l'assertion impactée
4. Variables one-hot des cycles comptables spécifiques
5. `priorite_audit` — le score composite de planification

Cette hiérarchie est cohérente avec la théorie de l'audit : le type de risque et les assertions touchées sont les deux critères primordiaux de l'évaluation du risque selon ISA 315.

---

## 9. Cartographie des risques et planification des missions

### 9.1 Matrice de risque (probabilité × impact)

Une cartographie bidimensionnelle des risques a été produite, positionnant chaque risque selon :
- **Axe X (probabilité)** : proxy construit à partir du type de risque, de l'assertion et de l'IRC
- **Axe Y (impact financier)** : proxy construit à partir du niveau de risque, de la complexité du cycle et du nombre de comptes PCG concernés

La matrice délimite trois zones conformes à la pratique d'audit ISA 315 :
- 🟢 **Zone acceptable** (faible probabilité × faible impact) : procédures analytiques minimales
- 🟡 **Zone à surveiller** (probabilité ou impact élevé) : tests de contrôle ciblés
- 🔴 **Zone critique** (forte probabilité × fort impact) : tests substantifs étendus et circularisation

### 9.2 Programme de travail d'audit

Un programme de travail structuré a été généré automatiquement, associant à chaque combinaison (Cycle × Niveau de risque × Assertion) :

| Niveau de risque | Procédure recommandée (ISA 330) | Budget estimé |
|:-:|---|:-:|
| 🔴 Élevé | Tests substantifs étendus + Circularisation + Analyse comparative détaillée (ISA 330.18) | 8h / risque |
| 🟡 Moyen | Tests de contrôle ciblés + Sondages sur pièces justificatives (ISA 330.8) | 4h / risque |
| 🟢 Faible | Procédures analytiques + Revue des contrôles existants (ISA 330.4) | 2h / risque |

### 9.3 Budget-temps total estimé

Le budget global de la mission s'établit à **1 026 heures**, réparties sur **73 procédures planifiées** selon la priorité d'intervention (P1 → P2 → P3).

### 9.4 Export des résultats

Les résultats sont exportés dans un fichier Excel structuré en trois feuilles (`resultats_audit_classification.xlsx`) :
- **Feuille 1** — Programme de travail d'audit (code couleur par niveau de risque)
- **Feuille 2** — Comparaison des modèles ML
- **Feuille 3** — Dataset enrichi avec toutes les features calculées

---

## 10. Discussion critique

### 10.1 Apports du projet

Ce projet démontre la **faisabilité technique** d'automatiser la classification des risques d'audit via le machine learning. Les principaux apports sont :

- La construction d'un pipeline ML complet et reproductible (de la donnée brute à l'export Excel professionnel)
- La création de features interprétables ancrées dans la normativité ISA, ouvrant la voie à une explicabilité du modèle aux équipes d'audit
- La génération automatique d'un programme de travail conforme aux pratiques OECCA Maroc
- La démonstration de la capacité prédictive sur un nouveau risque non vu (cellule de prédiction en temps réel)

### 10.2 Limites identifiées

**Limite 1 — Nature des données simulées.** Le dataset est construit par règles déterministes, ce qui explique les performances parfaites. Sur des données réelles (extraites de dossiers d'audit, d'ERP ou de systèmes comptables), les modèles devraient être réévalués avec une plus grande variabilité et des cas ambigus.

**Limite 2 — Overfitting apparent vs réel.** Les scores de 100% sont intrinsèquement liés à la structure du dataset. La validation croisée confirme leur stabilité (std = 0,000 pour 4 modèles), mais cela ne garantit pas la généralisation à des données hors-distribution.

**Limite 3 — Déséquilibre des cycles.** Le cycle Capitaux propres ne comprend que 10 observations contre 30 pour les autres cycles, ce qui pourrait nuire à la représentativité des apprentissages spécifiques à ce cycle dans un contexte réel.

**Limite 4 — Variables qualitatives non exploitées.** Les colonnes `Description du risque`, `Procédure d'audit` et `Documents de preuve` sont des champs textuels riches qui n'ont pas été valorisés dans ce projet. Des approches NLP (TF-IDF, embeddings) permettraient d'en extraire des features supplémentaires.

### 10.3 Pistes d'amélioration

- **Données réelles** : intégration de données anonymisées issues de dossiers d'audit pour tester la robustesse des modèles
- **NLP sur les descriptions** : analyse des libellés de risques par vectorisation textuelle
- **Optimisation hyperparamétrique** : GridSearchCV / RandomSearchCV pour optimiser les seuils des modèles
- **Modèles explicables (XAI)** : intégration de SHAP values pour justifier chaque prédiction auprès des équipes d'audit
- **Déploiement** : création d'une interface web (Streamlit) permettant à un auditeur de saisir un nouveau risque et d'obtenir une classification instantanée

---

## 11. Conclusion

Ce projet a permis de démontrer comment les techniques d'intelligence artificielle — et plus particulièrement la classification supervisée — peuvent être mobilisées au service de l'audit financier pour **automatiser, standardiser et objectiver la classification des risques** par cycle comptable.

En s'appuyant sur une base de données simulée conforme aux normes ISA et au PCG Maroc, le projet a traversé l'ensemble des étapes d'un projet IA : exploration des données, feature engineering ancré dans la théorie de l'audit, comparaison de cinq algorithmes (Régression Logistique, Decision Tree, Random Forest, Gradient Boosting, KNN), évaluation rigoureuse par validation croisée, cartographie des risques, et planification automatique des missions.

Les résultats obtenus — **100% d'accuracy et de F1-Score macro pour quatre modèles sur cinq** — reflètent la cohérence interne du dataset simulé et valident la pertinence des features construites. La Régression Logistique est retenue comme modèle de référence en raison de son interprétabilité, qualité indispensable dans le contexte réglementé de l'audit financier.

Sur le plan opérationnel, le pipeline produit génère automatiquement un **programme de travail d'audit de 1 026 heures** sur 73 procédures, directement exploitable par une équipe d'auditeurs, démontrant le potentiel transformatif de l'IA dans la planification des missions.

Ce travail constitue une base solide pour des développements futurs, notamment l'intégration de données réelles, l'exploitation des champs textuels par NLP, et le déploiement d'une solution opérationnelle dédiée aux cabinets d'audit marocains.

---

## 12. Références normatives

| Norme / Source | Titre | Application dans ce projet |
|---|---|---|
| **ISA 200** | Objectifs généraux de l'auditeur indépendant et réalisation d'un audit selon les ISA | Cadre conceptuel du risque d'audit |
| **ISA 240** | Responsabilités de l'auditeur concernant les fraudes | Pondération accrue des risques de fraude (score = 3) |
| **ISA 300** | Planification d'un audit d'états financiers | Budget-temps, programme de travail, priorités |
| **ISA 315** | Identification et évaluation des risques d'anomalies significatives | Feature engineering, assertions, cartographie |
| **ISA 330** | Réponses de l'auditeur aux risques évalués | Procédures recommandées par niveau de risque |
| **PCG Maroc** | Plan Comptable Général Marocain — OECCA | Numérotation des comptes (classes 3 à 7) |
| **scikit-learn** | Pedregosa et al. (2011) — *JMLR* | Framework ML : modèles, métriques, validation |
| **pandas / numpy** | McKinney (2010) / Harris et al. (2020) | Manipulation et calcul sur les données |
| **matplotlib / seaborn** | Hunter (2007) / Waskom (2021) | Visualisations et analyses graphiques |

---

*Compte rendu rédigé sur la base du notebook `Projet_IA_CAC2_IZLITNE_MAROUANE_ET_NOUBAIR_ZIAD.ipynb` et des données `base_donnees_risques_audit_cycle_comptable.xlsx`*
*Marrakech, Mars 2026 — IZLITNE Marouane & NOUBAIR Ziad*
