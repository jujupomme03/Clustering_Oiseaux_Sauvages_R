# Classification non supervisée - Charactéristiques physiques d'oiseaux sauvage

## Description

Ce projet explore dans quelle mesure les **caractéristiques morphologiques osseuses** de 420 oiseaux permettent de distinguer automatiquement leurs six types écologiques, en appliquant trois méthodes de **classification non supervisée** : CAH, K-means et GMM.

La variable `type` (étiquette écologique) est volontairement ignorée lors de la classification, et sert uniquement à **évaluer a posteriori** la qualité des partitions obtenues.

---

## Données

Le jeu de données `bird.csv` contient **420 observations** et **12 variables** :

| Variable | Description |
|---|---|
| `id` | Identifiant de l'individu |
| `huml` / `humw` | Longueur / Largeur de l'humérus (mm) |
| `ulnal` / `ulnaw` | Longueur / Largeur de l'ulna (mm) |
| `feml` / `femw` | Longueur / Largeur du fémur (mm) |
| `tibl` / `tibw` | Longueur / Largeur du tibia (mm) |
| `tarl` / `tarw` | Longueur / Largeur du tarse (mm) |
| `type` | Type écologique (6 modalités) |

Les six types écologiques sont : **P** (Percheur), **R** (Rapace), **SO** (Chanteur), **SW** (Nageur), **T** (Terrestre), **W** (Échassier).

---

## Méthodologie

### 1. Pré-traitement
- Suppression des valeurs manquantes et aberrantes (420 → 413 observations)
- Standardisation (centrage-réduction) des 10 variables morphologiques

### 2. Analyse exploratoire — ACP
- L'axe 1 concentre **85,4 %** de la variance totale
- Toutes les variables contribuent quasi-uniformément à cet axe → signal de **taille corporelle globale**
- Structure quasi-unidimensionnelle, ellipses fortement chevauchantes

### 3. Méthodes de classification

| Méthode | Outil R | Critère de sélection de K |
|---|---|---|
| Classification Ascendante Hiérarchique (CAH) | `hclust` (Ward) | Dendrogramme + NbClust |
| K-means | `kmeans` (nstart = 50) | Méthode du coude |
| Modèle de Mélanges Gaussiens (GMM) | `Mclust` | BIC |

### 4. Évaluation
- **Adjusted Rand Index (ARI)** pour comparer les partitions aux vraies étiquettes
- Description morphologique des clusters obtenus

---

## Résultats

| Méthode | K | ARI |
|---|---|---|
| CAH | 2 | 0.160 |
| CAH | 6 | 0.203 |
| K-means | 2 | 0.107 |
| K-means | 6 | 0.241 |
| GMM (G optimal) | 9 | 0.260 |
| **GMM** | **2** | **0.308** |
| GMM | 6 | 0.261 |

**Enseignements clés :**
- Les trois méthodes convergent vers **K = 2** comme partition optimale, opposant *petits oiseaux* (SO, P, T) aux *grands oiseaux* (SW, R, W)
- Forcer K = 6 produit 6 tranches de gabarit plutôt que 6 types écologiques distincts
- Les chanteurs (SO) sont l'exception : leur très petite taille les rend systématiquement bien isolés
- Les mesures osseuses brutes ne sont pas suffisamment discriminantes pour reconstituer les 6 catégories écologiques

---

## Technologies

- **Langage :** R
- **Packages :** `tidyverse`, `cluster`, `factoextra`, `NbClust`, `mclust`
- **Rapport :** R Markdown → HTML


