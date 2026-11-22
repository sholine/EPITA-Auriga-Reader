# Auriga EPITA – noms d'épreuves lisibles

Script utilisateur (Tampermonkey) qui rend la page **« Ses notes (éval) »** d'Auriga, l'outil de notes d'EPITA, compréhensible par des humains — et en particulier par les parents.

## Le problème

Auriga affiche les épreuves avec une nomenclature interne difficile à lire :

```
2627_I_INF_FISE_S01_CN_PC_DP_EXA_1
```

## La solution

Le script remplace automatiquement ces codes par un libellé clair :

| Avant | Après |
|---|---|
| `2627_I_INF_FISE_S01_CN_PC_DP_EXA_1` | S1 · Concevoir – Maths – Dénombrement et probabilités · Examen n°1 |
| `2627_I_INF_FISE_S01_PR_PET_FAF` | S1 · Produire – Python et environnement de travail · TP en conditions d'examen |
| `2627_I_INF_FISE_S01_AG_ACF_EXO_1` | S1 · Agir – Anglais communication formelle · Expression écrite en classe n°1 |

Il apporte aussi deux petits conforts :

- **Notes arrondies** : `15.3333333333` → `15,33`
- **Coefficients en pourcentage** : `90,00` → `90%`

Le code d'origine reste visible en passant la souris sur le libellé (ou sur la note arrondie).

Le script ne fait que **modifier l'affichage dans votre navigateur** : il n'envoie aucune donnée, ne modifie rien sur Auriga et ne fonctionne que sur `auriga.epita.fr`.

## Installation (Chrome)

1. Installez l'extension [Tampermonkey](https://chromewebstore.google.com/detail/tampermonkey/dhdgffkkebhmkfjojejmpbldmpobfkfo).
2. Dans `chrome://extensions`, activez le **Mode développeur** (en haut à droite), puis ouvrez les **Détails** de Tampermonkey et activez **« Autoriser les scripts utilisateur »** (exigé par les versions récentes de Chrome).
3. Ouvrez le fichier [`auriga-epreuves-lisibles.user.js`](auriga-epreuves-lisibles.user.js) dans ce dépôt et cliquez sur **Raw** : Tampermonkey propose alors l'installation.
4. Rechargez la page des notes sur Auriga.

Fonctionne aussi avec Firefox, Edge ou Safari via Tampermonkey ou Violentmonkey.

## Comment lire un code

```
2627 _ I _ INF _ FISE _ S01 _ CN _ PC _ DP _ EXA _ 1
 │     │    │     │      │     │    │    │    │    └ numéro de l'épreuve
 │     │    │     │      │     │    │    │    └ type d'épreuve
 │     │    │     │      │     │    │    └ matière (ECUE)
 │     │    │     │      │     │    └ parcours (PC / PA) – masqué
 │     │    │     │      │     └ UE : Agir, Concevoir, Piloter, Produire
 │     │    │     │      └ semestre
 │     │    │     └ Formation Initiale Sous Statut Étudiant
 │     │    └ Informatique
 │     └ cycle Ingénieur
 └ promotion 2026-2027
```

## Adapter ou compléter le dictionnaire

Toute la correspondance code → libellé se trouve dans trois blocs en haut du script :

- `MATIERES` : UE et matières (`DP: 'Maths – Dénombrement et probabilités'`). Une valeur vide `''` masque le code.
- `TYPES` : types d'épreuve (`EXA`, `EXF`, `EXO`, `DMA`, `FAF`…).
- `TYPES_PAR_MATIERE` : exceptions quand un même type n'a pas le même sens selon la matière (ex. `EXO` en anglais = expression écrite en classe).

Un code absent du dictionnaire s'affiche tel quel : il suffit d'ajouter une ligne.

Deux options se désactivent en passant leur valeur à `false` : `ARRONDIR_NOTES` et `COEF_EN_POURCENT`.

## Limites connues

- Le dictionnaire a été établi pour le **semestre 1 du cycle ingénieur informatique (parcours PC/PA), promotion 2026-2027**. Les autres semestres, parcours ou campus utilisent sans doute d'autres codes.
- Si Auriga change sa présentation ou son adresse, le script peut nécessiter une mise à jour (lignes `@match` en tête de fichier).

## Contribuer

Les contributions sont les bienvenues, en particulier pour **compléter le dictionnaire** des autres semestres et parcours. Ouvrez une *issue* avec les codes et leur signification, ou proposez directement une *pull request*.

⚠️ Merci de **ne jamais publier de captures contenant des noms d'étudiants ou des notes réelles**.

## Licence

Distribué sous licence [MIT](LICENSE) : libre d'utilisation, de modification et de redistribution, y compris dans d'autres projets, à condition de conserver la mention de licence.

Projet indépendant, non affilié à EPITA ni à l'éditeur d'Auriga.
