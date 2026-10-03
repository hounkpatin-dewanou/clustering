# Segmentation de céréales par K-Means (WEKA & Python)

Projet de clustering non supervisé appliquant l'algorithme **K-Means** au jeu de données **Cereals** (77 céréales, 16 attributs) afin d'identifier des groupes de produits aux profils nutritionnels similaires. L'étude est réalisée dans deux environnements, **WEKA** puis **Python (scikit-learn)**, et compare deux solutions : **k = 3** et **k = 5**.

> Travail réalisé dans le cadre d'un TP de segmentation — Université d'Abomey-Calavi (Bénin), année académique 2025-2026.

## Objectifs

- Segmenter le marché des céréales à partir de leurs caractéristiques nutritionnelles et commerciales.
- Comparer les solutions k = 3 et k = 5 et déterminer la plus satisfaisante.
- Vérifier que l'implémentation Python reproduit la configuration utilisée dans WEKA.

## Structure du dépôt

| Fichier | Description |
|---|---|
| `cereals.csv` | Jeu de données (77 instances, 16 attributs) |
| `clustering.py` | Script Python : prétraitement, K-Means (k = 5 et k = 3), SSE, silhouette |
| `cereals_results_k3.txt` | Sortie brute WEKA pour k = 3 |
| `cereals_results_k5.txt` | Sortie brute WEKA pour k = 5 |
| `SEGMENTATION.docx` | Énoncé du TP |
| `WEKA-K-means .docx` / `.pdf` | Rapport de l'étude réalisée avec WEKA |
| `Python-K-means.docx` / `.pdf` | Rapport de l'étude réalisée avec Python |

## Données

Le fichier `cereals.csv` contient 77 céréales décrites par :

- **Attributs ignorés** (non numériques) : `name`, `mfr`, `type`
- **13 attributs utilisés** : `calories`, `protein`, `fat`, `sodium`, `fiber`, `carbo`, `sugars`, `potass`, `vitamins`, `shelf`, `weight`, `cups`, `rating`

## Méthodologie

1. **Imputation des valeurs manquantes** par la moyenne (équivalent du filtre WEKA `ReplaceMissingValues`).
2. **Normalisation Min-Max** dans [0, 1] (équivalent du filtre WEKA `Normalize`).
3. **K-Means** avec distance euclidienne, initialisation aléatoire, 500 itérations maximum et graine fixée à 10 (paramètres alignés sur WEKA : `-I 500 -S 10`).
4. **Évaluation** par l'inertie intra-cluster (SSE) et le score de silhouette.

## Installation et exécution

Prérequis : Python 3.8+

```bash
git clone https://github.com/hounkpatin-dewanou/clustering.git
cd clustering
pip install pandas numpy scikit-learn
python clustering.py
```

Le script affiche, pour k = 5 puis k = 3 : les moyennes des attributs par cluster, la répartition des instances, le SSE et le score de silhouette, puis une comparaison finale.

Pour reproduire l'étude avec WEKA : charger `cereals.csv`, appliquer `ReplaceMissingValues` puis `Normalize`, ignorer les attributs `name`, `mfr` et `type`, et lancer `SimpleKMeans` avec `-N 3` ou `-N 5`, `-I 500` et `-S 10`.

## Résultats

| Environnement | k | SSE | Silhouette |
|---|---|---|---|
| Python | 5 | 24,10 | 0,277 |
| Python | 3 | 32,87 | 0,242 |
| WEKA | 5 | 25,71 | n/a |
| WEKA | 3 | 34,79 | n/a |

Les valeurs diffèrent légèrement entre WEKA et Python (initialisation aléatoire et implémentations différentes), mais les tendances sont identiques.

### Répartition des instances (Python)

- **k = 5** : 13 / 10 / 3 / 33 / 18 céréales
- **k = 3** : 15 / 35 / 27 céréales

### Interprétation des 5 clusters (Python)

| Cluster | Profil |
|---|---|
| 0 | Énergétique : apport maximal en glucides |
| 1 | Santé naturelle : très pauvre en sodium et en sucre |
| 2 | Diététique : riche en fibres et potassium (meilleur rating) |
| 3 | Moyen : céréales enrichies en vitamines, étagère du haut |
| 4 | Moins sains : très sucrées, pauvres en nutriments essentiels |

## Conclusion

La solution **k = 5** est jugée la plus satisfaisante : elle offre un SSE plus faible, un meilleur score de silhouette et une interprétation nutritionnelle plus fine que k = 3, qui reste trop généraliste. Les rapports détaillés (`.docx` / `.pdf`) présentent l'analyse complète.

## Auteurs

Groupe 1 :

- BAWA SACCA Hamid
- COCOUVI Alexandro
- HOUNKPATIN Dèwanou Hugues-Marie
- OUSSA Chadrac Espoir
- PATINDE Nolan

## Source des données

Jeu de données *Cereals* (Kaggle / UCI Machine Learning Repository).
