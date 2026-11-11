# TP1 – Clustering des graines de courge

Notebook : `ANADON_TP1_complet.ipynb`

Analyse non supervisée de 2 500 graines de courge décrites par 12 mesures géométriques, avec trois méthodes (K-means, clustering hiérarchique, clustering spectral). La colonne `Class` du jeu de données n'est **jamais utilisée** : elle est retirée dès l'import.

---

## 1. Où placer le fichier Excel

Le notebook lit le jeu de données original (*Pumpkin Seeds Dataset*, Koklu, Sarigil et Ozbek, 2021). Il faut le placer **à côté du notebook**, dans un dossier nommé `Pumpkin_Seeds_Dataset` :

```
Dossier_du_projet/
├── ANADON_TP1_complet.ipynb
├── README.md
└── Pumpkin_Seeds_Dataset/
    └── Pumpkin_Seeds_Dataset.xlsx      <-- le fichier Excel ici
```

Le notebook accepte aussi le fichier directement à côté du notebook (`Pumpkin_Seeds_Dataset.xlsx`, sans sous-dossier). Il cherche dans cet ordre, et prend le premier fichier trouvé :

1. `pumpkin_selected_12_raw.csv` (CSV commun du groupe, voir partie 3)
2. `Pumpkin_Seeds_Dataset.csv`
3. `Pumpkin_Seeds_Dataset/Pumpkin_Seeds_Dataset.csv`
4. `Pumpkin_Seeds_Dataset/Pumpkin_Seeds_Dataset.xlsx`
5. `Pumpkin_Seeds_Dataset.xlsx`

Si aucun de ces fichiers n'existe, le notebook s'arrête avec un message qui liste ce qu'il attendait.

**Contenu attendu du fichier.** Les 12 colonnes suivantes, avec exactement ces noms (la colonne `Class` peut être présente, elle sera supprimée) :

`Area`, `Perimeter`, `Major_Axis_Length`, `Minor_Axis_Length`, `Convex_Area`, `Equiv_Diameter`, `Eccentricity`, `Solidity`, `Extent`, `Roundness`, `Aspect_Ration`, `Compactness`

Une vérification (`assert`) dans le notebook échoue si les colonnes sont différentes. Pour un fichier Excel, la lecture se fait sur la **première feuille**.

**Important.** Les chemins sont relatifs au dossier de travail du noyau. Lance le notebook depuis le dossier du projet (c'est le cas par défaut dans Jupyter). Dans VS Code, vérifie que le dossier ouvert est bien celui du projet.

---

## 2. Lancer le notebook

### Première cellule de code : installation des bibliothèques

Nous avons mis **en tout début de notebook** (section « 0. Installation et import des bibliothèques ») une petite cellule qui :

- vérifie que les bibliothèques nécessaires sont installées : `numpy`, `pandas`, `matplotlib`, `seaborn`, `scikit-learn`, `scipy` et `openpyxl` (cette dernière sert à lire le fichier Excel) ;
- **installe automatiquement avec `pip` celles qui manquent** ;
- affiche « Toutes les bibliothèques nécessaires sont déjà installées » s'il n'y a rien à faire.

Si une erreur d'import apparaît juste après une installation, il suffit de **redémarrer le noyau** puis de relancer.

### Ensuite : Run All

Comme cette cellule d'installation est la première, il suffit de faire **Run All** (« Exécuter toutes les cellules ») pour que tout s'enchaîne dans le bon ordre :

1. installation / vérification des bibliothèques ;
2. imports ;
3. chargement des données et pré-traitement ;
4. K-means, clustering hiérarchique, clustering spectral ;
5. comparaison des trois méthodes et conclusion.

Les cellules dépendent les unes des autres : ne pas les exécuter dans le désordre. En cas de doute, redémarre le noyau puis relance un Run All complet. Le calcul prend quelques minutes (clustering spectral et tests de stabilité). Les résultats sont reproductibles : la graine aléatoire est fixée (`RS = 0`).

---

## 3. Les fichiers CSV créés par le notebook

Le notebook **écrit** quatre fichiers CSV à côté de lui. Ce sont des exports de travail (le reste de l'analyse se fait en mémoire) : ils servent à partager et à reproduire les données exactement telles qu'elles ont été utilisées.

| Fichier | Contenu | À quoi il sert |
|---|---|---|
| `pumpkin_selected_12_raw.csv` | Les **12 variables brutes** (unités d'origine), sans `Class`. | Fichier de départ commun au groupe : le notebook le relit en priorité à la place de l'Excel. |
| `pumpkin_selected_8_raw.csv` | Les **8 variables retenues**, brutes, sans standardisation. | Interpréter les groupes dans les unités d'origine (moyennes, tailles, rapports). |
| `pumpkin_preprocessed_12.csv` | Les 12 variables **standardisées** (moyenne 0, écart-type 1). | Données utilisées pour les calculs de la comparaison « 12 variables ». |
| `pumpkin_preprocessed_8.csv` | Les 8 variables **standardisées**. | Données de l'**analyse principale** (K-means, Ward, spectral) : c'est la matrice sur laquelle se fait le clustering. |

Les variables écartées pour passer de 12 à 8 sont `Convex_Area`, `Equiv_Diameter`, `Eccentricity` et `Compactness` (très corrélées à d'autres mesures).

**À savoir :**

- Les fichiers *raw* servent à **interpréter** (valeurs réelles), les fichiers *preprocessed* servent à **calculer** (variables à la même échelle).
- Ces CSV sont recréés à chaque exécution. Le notebook ne relit que `pumpkin_selected_12_raw.csv`.
- Si tu changes le jeu de données (autre Excel), **supprime d'abord `pumpkin_selected_12_raw.csv`**, sinon le notebook continuera à lire l'ancien CSV au lieu du nouveau fichier.

---

## 4. Problèmes fréquents

| Message / symptôme | Cause probable | Solution |
|---|---|---|
| `FileNotFoundError: Aucun fichier de données trouvé` | Excel absent ou mal placé | Reprendre la partie 1 (nom du dossier et du fichier exacts). |
| `Colonnes inattendues dans le fichier de données` | Le fichier n'a pas les 12 colonnes attendues | Utiliser le fichier original du jeu de données. |
| `ModuleNotFoundError` après l'installation | Le noyau ne voit pas encore le nouveau paquet | Redémarrer le noyau, puis Run All. |
| Une cellule plante en milieu de notebook | Cellules lancées dans le désordre | Redémarrer le noyau, puis Run All. |
