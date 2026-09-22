# Mesures DAX — SAP CO Dashboard

Ce dossier regroupe les principales mesures DAX utilisées pour construire le dashboard SAP CO.
Les mesures permettent de suivre le budget, les dépenses réelles, les écarts et leur niveau de significativité.

---

## 1. KPI principaux

### Budget Total

Calcule le montant total du budget.

```DAX
Budget Total =
SUM('Données'[Budget_EUR])
```

### Réel Total

Calcule le montant total des dépenses réellement enregistrées.

```DAX
Reel Total =
SUM('Données'[Reel_EUR])
```

### Nombre de dépenses

Compte le nombre de lignes de dépenses présentes dans les données.

```DAX
Nombre Depenses =
COUNTROWS('Données')
```

---

## 2. Analyse des écarts

### Écart Total (€)

Calcule la différence entre les dépenses réelles et le budget.

**Formule métier :**

> Écart = Réel − Budget

```DAX
Ecart Total =
[Reel Total] - [Budget Total]
```

Interprétation :

* Écart positif → dépenses supérieures au budget → **Défavorable**
* Écart négatif → dépenses inférieures au budget → **Favorable**
* Écart nul → dépenses conformes au budget

---

### Écart en pourcentage

Mesure l'écart par rapport au budget.

**Formule métier :**

> Écart % = (Réel − Budget) / Budget

```DAX
Ecart_Pct =
DIVIDE(
    [Ecart Total],
    [Budget Total],
    0
)
```

La mesure est formatée en **Pourcentage (%)** dans Power BI.

Exemple :

> `0,1637` → `16,37 %`

---

## 3. Analyse de la significativité

### Écart significatif

Un écart est considéré comme significatif lorsque les deux conditions suivantes sont respectées :

* Écart en euros > 10 000 €
* Écart en pourcentage > 10 %

```DAX
Ecart_Significatif =
IF(
    [Ecart Total] > 10000
        && [Ecart_Pct] > 0.10,
    "⚠ Écart significatif",
    "✓ Écart non significatif"
)
```

Cette mesure permet d'identifier rapidement les dépassements budgétaires importants.

---

## 4. Statut de l'écart

### Statut Écart

Détermine automatiquement si l'écart est favorable, défavorable ou conforme.

```DAX
Statut_Ecart =
SWITCH(
    TRUE(),
    [Ecart Total] > 0, "▲ Défavorable",
    [Ecart Total] < 0, "▼ Favorable",
    "● Conforme"
)
```

### Règle d'interprétation

| Situation | Interprétation |
| --------- | -------------- |
| Écart > 0 | ▲ Défavorable  |
| Écart < 0 | ▼ Favorable    |
| Écart = 0 | ● Conforme     |

---

## 5. Mise en forme conditionnelle

### Couleur du statut

Cette mesure fournit automatiquement une couleur en fonction du statut de l'écart.

```DAX
Couleur_Statut =
SWITCH(
    TRUE(),
    [Ecart Total] > 0, "#D32F2F",
    [Ecart Total] < 0, "#2E7D32",
    "#757575"
)
```

Correspondance :

| Situation   | Couleur | Code      |
| ----------- | ------- | --------- |
| Défavorable | Rouge   | `#D32F2F` |
| Favorable   | Vert    | `#2E7D32` |
| Conforme    | Gris    | `#757575` |

Cette mesure est utilisée avec la **mise en forme conditionnelle** des visuels Power BI.

---

## 6. Utilisation dans les visuels

Les mesures DAX sont utilisées dans différents éléments du dashboard.

### Cartes KPI

* `Budget Total`
* `Reel Total`
* `Nombre Depenses`
* `Ecart Total`
* `Ecart_Pct`

### Indicateurs sous les KPI

* `Statut_Ecart`
* `Ecart_Significatif`

### Graphique Budget vs Réel par mois

* `Budget Total`
* `Reel Total`
* `Ecart Total`

L'axe temporel utilise le champ `Mois`.

Les mesures étant calculées dynamiquement, elles s'adaptent automatiquement aux filtres appliqués dans le rapport : mois, Business Unit, centre de coût, nature de dépense, etc.

---

## 7. Synthèse de la logique métier

Le dashboard repose principalement sur la logique suivante :

```text
                 DONNÉES SAP CO
                       │
             ┌─────────┴─────────┐
             │                   │
          BUDGET                RÉEL
             │                   │
             └─────────┬─────────┘
                       │
                  ÉCART TOTAL
                  Réel - Budget
                       │
             ┌─────────┴─────────┐
             │                   │
          Écart €             Écart %
             │                   │
             └─────────┬─────────┘
                       │
              Analyse du statut
                       │
          ┌────────────┼────────────┐
          │            │            │
      Favorable   Défavorable    Conforme
                       │
               Test significatif
                       │
             > 10 000 € ET > 10 %
```

---

## 8. Mesures principales — Vue d'ensemble

| Mesure               | Objectif                           |
| -------------------- | ---------------------------------- |
| `Budget Total`       | Total du budget                    |
| `Reel Total`         | Total des dépenses réelles         |
| `Nombre Depenses`    | Nombre de dépenses                 |
| `Ecart Total`        | Différence Réel − Budget           |
| `Ecart_Pct`          | Écart relatif au budget            |
| `Ecart_Significatif` | Détection des écarts importants    |
| `Statut_Ecart`       | Favorable / Défavorable / Conforme |
| `Couleur_Statut`     | Couleur dynamique du statut        |

---

## Conclusion

Les mesures DAX permettent de transformer les données financières SAP CO en indicateurs de pilotage.

Elles permettent notamment de :

* suivre le budget et les dépenses réelles ;
* mesurer les écarts en valeur et en pourcentage ;
* identifier les dépassements budgétaires importants ;
* qualifier automatiquement les écarts ;
* adapter les indicateurs aux filtres du dashboard ;
* améliorer la lecture visuelle grâce à la mise en forme conditionnelle.
