# Algèbre relationnelle en OCaml

Implémentation en OCaml d'une algèbre relationnelle en mémoire, avec analyse des dépendances fonctionnelles et calcul du niveau de normalisation d'une table.

Projet d'école (ENSIIE, cours de programmation fonctionnelle), réalisé seul à partir du typage donné avec une liste de fonctions à implémenter.

## Fonctionnalités

**Modèle de données**
- Tables typées : entiers (`TInt`), textes (`TText`), valeur `NULL` autorisée ou non par colonne
- Validation d'une table (`check_table`) : taille des lignes, typage, unicité des noms de colonnes

**Opérations relationnelles**
- `insert` : insertion d'une ligne avec vérification du schéma
- `prod` : produit cartésien (renommage automatique des colonnes en doublon)
- `projection` : projection sur une liste de champs
- `restrict` : sélection par prédicat, avec des prédicats prêts à l'emploi champ-valeur (`test_eq_fv`, `test_lt_fv`, ...) et champ-champ (`test_eq_ff`, ...)

**Dépendances fonctionnelles**
- `compute_deps` : toutes les dépendances fonctionnelles d'une table
- `compute_elementary_deps` : dépendances élémentaires
- `list_all_keys` : clés candidates
- `normalization_level` : niveau de normalisation (1NF, 2NF ou 3NF)

## Compilation et tests

Prérequis : `ocaml` et `ocamlc`.

```bash
make        # compile
make run    # lance les tests
make clean  # nettoie
```

La sortie attendue :

```
Début des tests...
check_table : ok
insert : ok
prod : ok
projection : ok
restrict : ok
compute_deps : ok
compute_elementary_deps : ok
normalization_level : ok
```

## Exemple

```ocaml
let personnages = {
  cols = [("nom", (TText, false)); ("hp", (TInt, true))];
  rows = [
    [VText "Ness"; VInt 100];
    [VText "Lucas"; VInt 60];
  ]
};;

(* lignes dont hp > 80 *)
let sacs = restrict personnages (test_gt_fv "hp" (VInt 80));;

(* projection sur le nom *)
let noms = projection sacs ["nom"];;
```

## Choix de conception

- Choix du sujet donné : une table est un schéma (liste de couples nom/type) et une liste de lignes.
- Chaque fonction documente son contrat (`@requires`, `@ensures`, `@raises`).
- Les erreurs sont signalées par des exceptions : `Invalid_table`, `Invalid_row_size`, `Invalid_row_type`, `Invalid_field`.
- Les tests sont des `assert` couvrant les tables invalides, les tables vides et les cas limites.

## Limites

Tout est en mémoire et aucune interface d'entrée et sortie de fichiers.
Le calcul des dépendances fonctionnelles est exponentiel en nombre de colonnes et quadratique en nombre de lignes.
Optimisations possibles : élagage des candidats, structures de données autres que les listes chaînées.

## Structure

```
projet.ml   # implémentation et tests
Makefile
```
