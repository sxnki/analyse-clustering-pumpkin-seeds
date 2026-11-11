# Analyse de données - Clustering de graines de courge

Projet académique réalisé dans le cadre du TP1 d'analyse de données. L'objectif est d'identifier une structure naturelle dans un jeu de données décrivant **2 500 graines de courge** à partir de mesures géométriques.

## Objectifs

L'étude compare trois méthodes de clustering non supervisé :

- **K-means** ;
- **clustering hiérarchique agglomératif** avec plusieurs stratégies de liaison ;
- **clustering spectral** fondé sur un graphe de voisinage.

La colonne `Class` correspond à une étiquette connue du jeu de données. Elle est supprimée et n'est pas utilisée pour construire, choisir ou évaluer les groupes.

## Résultats principaux

Les trois méthodes font apparaître deux profils géométriques principaux :

- des graines plutôt trapues et arrondies ;
- des graines plutôt allongées.

La séparation reste progressive : les groupes décrivent des tendances et non deux catégories parfaitement isolées.

| Méthode  | Groupes | Silhouette | Calinski-Harabasz |
| -------- | ------: | ---------: | ----------------: |
| K-means  |       2 |      0,306 |           1 106,5 |
| Ward     |       2 |      0,231 |             810,2 |
| Spectral |       2 |      0,282 |           1 056,0 |

La partition retenue est celle de **K-means avec 2 groupes et 8 variables**. Le clustering spectral confirme globalement cette structure, tandis que l'approche hiérarchique apporte une lecture visuelle grâce au dendrogramme.

## Données et prétraitement

Les données contiennent 12 mesures morphologiques. Une représentation à 8 variables est utilisée pour l'analyse principale afin de limiter la redondance entre variables fortement corrélées :

`Area`, `Perimeter`, `Major_Axis_Length`, `Minor_Axis_Length`, `Solidity`, `Extent`, `Roundness`, `Aspect_Ration`.

Les variables sont standardisées avant les calculs de distance. Deux versions sont conservées :

- **12 variables**, pour la comparaison ;
- **8 variables**, pour l'analyse principale.

## Contenu du dépôt

```text
.
├── ANADON_TP1_complet/
│   ├── ANADON_TP1_complet.ipynb
│   ├── README.md
│   ├── Pumpkin_Seeds_Dataset/
│   │   ├── Pumpkin_Seeds_Dataset.arff
│   │   └── Pumpkin_Seeds_Dataset.xlsx
│   └── *.csv
└── README.md
```

Le notebook du TP1 se trouve dans [`ANADON_TP1_complet/ANADON_TP1_complet.ipynb`](./ANADON_TP1_complet/ANADON_TP1_complet.ipynb).
Le fichier Excel original utilisé par le notebook se trouve dans [`ANADON_TP1_complet/Pumpkin_Seeds_Dataset/Pumpkin_Seeds_Dataset.xlsx`](./ANADON_TP1_complet/Pumpkin_Seeds_Dataset/Pumpkin_Seeds_Dataset.xlsx).

## Installation et exécution

Installer Python 3 ainsi que Jupyter Notebook ou utiliser l'extension Jupyter de VS Code. Les bibliothèques principales sont :

```bash
pip install numpy pandas matplotlib seaborn scikit-learn scipy jupyter openpyxl
```

Puis ouvrir le notebook et exécuter les cellules dans l'ordre, idéalement avec **Run All**.

Les fichiers CSV présents dans le dossier du TP1 permettent de reproduire l'analyse déjà préparée. Pour plus de détails sur les fichiers attendus et les problèmes fréquents, consulter [`README.md`](./ANADON_TP1_complet/README.md).

## Jeu de données et citation

> Koklu, M., Sarigil, S., & Ozbek, O. (2021). _The use of machine learning methods in classification of pumpkin seeds (Cucurbita pepo L.)_. Genetic Resources and Crop Evolution, 68(7), 2713–2726.  
> https://doi.org/10.1007/s10722-021-01226-0

Jeu de données : [muratkoklu.com/datasets](https://www.muratkoklu.com/datasets/).

## Auteurs

Projet réalisé par MEZIANE dans le cadre du Master 1.

## Licence

##### Ce dépôt est destiné à un usage académique. Les données restent soumises aux conditions de leur source originale.
