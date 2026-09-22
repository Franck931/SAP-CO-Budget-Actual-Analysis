# SAP CO — Analyse Budgétaire

## 📊 Présentation

Projet Power BI consacré au suivi et à l'analyse des écarts entre
les budgets et les dépenses réelles dans un environnement SAP CO.

L'objectif est de permettre une lecture rapide de la performance
budgétaire par mois, Business Unit, centre de coût et nature de dépense.

## 🎯 Objectifs

- Suivre le budget et le réel
- Identifier les écarts budgétaires
- Analyser les écarts par nature de dépense
- Identifier les centres de coûts présentant les principaux écarts
- Détecter les écarts significatifs
- Faciliter le pilotage budgétaire

## 🛠️ Technologies

- Power BI
- DAX
- Excel
- Power Query
- SAP CO

## 📌 Indicateurs principaux

- Budget total
- Réel total
- Écart total
- Écart %
- Nombre d'écarts significatifs

## 🚨 Règle de détection

Un écart est considéré comme significatif lorsque :

Écart € > 10 000 €

ET

Écart % > 10 %

## 📈 Analyses

Le dashboard permet notamment d'analyser :

- les performances mensuelles
- les écarts par nature de dépense
- les écarts par centre de coût
- les Business Units
- les lignes présentant des écarts significatifs

## 🎨 Dashboard

![SAP CO Dashboard](Screenshots/SAP_CO_Dashboard.png)

## 📁 Structure du projet

```text
SAP-CO-Budget-Actual-Analysis/
├── README.md
├── PowerBI/
├── Data/
├── Screenshots/
└── DAX/