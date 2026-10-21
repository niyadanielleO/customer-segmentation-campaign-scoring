# Note de cadrage — Segmentation clients et ciblage des campagnes

**Projet :** Segmentation et score d'appétence d'une base clients retail
**Auteure :** Niya C. Danielle Ouattara
**Version :** 1.0 — octobre 2026

---

## 1. Contexte

Une enseigne de distribution alimentaire vend en magasin, sur son site web et par catalogue. Ses clients achètent des produits frais (viande, poisson, fruits), des vins, des produits sucrés et des articles premium.

Au cours des deux dernières années, l'enseigne a mené six campagnes marketing, avec des taux d'acceptation très variables : de 1 à 8 % environ pour les cinq premières, autour de 15 % pour la dernière. On part de l'hypothèse que chaque campagne a été envoyée à l'ensemble de la base, sans ciblage. Les taux exacts seront recalculés dans l'analyse.

La direction marketing veut savoir si elle peut obtenir les mêmes résultats en contactant moins de clients, et mieux connaître sa clientèle pour adapter ses offres.

## 2. Questions posées

1. **Qui sont nos clients ?** Quels profils d'achat se dégagent, selon les canaux, les catégories de produits et la composition du foyer ?
2. **Quels segments activer ?** Peut-on regrouper la base en quatre à cinq segments homogènes, compréhensibles et actionnables par les équipes ?
3. **Qui cibler à la prochaine campagne ?** Peut-on estimer, pour chaque client, sa probabilité de répondre ?
4. **Combien ça rapporte ?** Quel résultat financier obtient-on en ciblant les clients les mieux scorés, par rapport à un envoi à toute la base ?

## 3. Périmètre et données

| Élément | Détail |
|---|---|
| Source | *Customer Personality Analysis* — Kaggle |
| Lien | https://www.kaggle.com/datasets/imakash3011/customer-personality-analysis |
| Volume | 2 240 clients, 29 variables |
| Période | Clients entrés entre juillet 2012 et juin 2014 ; dépenses cumulées sur deux ans |
| Familles de variables | Profil (âge, éducation, situation familiale, revenu, enfants), dépenses par catégorie, achats par canal, réponses aux six campagnes |

La devise n'est pas précisée dans le jeu de données : les montants sont exprimés en unités monétaires (u.m.).

## 4. Hypothèses économiques

Le jeu de données fournit deux constantes, utilisées telles quelles pour la simulation :

| Hypothèse | Valeur | Source |
|---|---|---|
| Coût d'un contact | 3 u.m. | colonne `Z_CostContact` |
| Revenu généré par une réponse positive | 11 u.m. | colonne `Z_Revenue` |

Ces hypothèses seront testées à la hausse et à la baisse dans la simulation, pour mesurer la sensibilité des conclusions.

## 5. Indicateurs de réussite

| Indicateur | Définition |
|---|---|
| Taux de réponse | Réponses positives / clients contactés |
| Part des répondants captée | Répondants atteints par le ciblage / répondants totaux |
| Résultat de la campagne | Réponses × 11 − contacts × 3 |
| Retour sur investissement | Résultat / coût total des contacts |
| Qualité du score | AUC et courbe de gains |

## 6. Démarche

| Étape | Livrable |
|---|---|
| Préparation des données | Base nettoyée et enrichie — notebook `01_preparation` |
| Analyse des comportements | Profils d'achat par canal, catégorie et foyer — notebook `02_comportements` |
| Segmentation | Quatre à cinq segments nommés et décrits — notebook `03_segmentation` |
| Score d'appétence | Modèle de réponse, AUC, courbe de gains — notebook `04_score` |
| Performance et ROI | Taux d'acceptation par segment, simulation de ciblage — notebook `05_roi` |
| Restitution | Dashboard Tableau Public et présentation de 8 à 10 pages |

## 7. Limites connues

- Les données sont anciennes (2012-2014) et proviennent d'un seul jeu public : les conclusions illustrent une méthode, elles ne décrivent pas un marché actuel.
- La réponse mesurée est celle de la dernière campagne ; le score suppose que la prochaine campagne sera comparable.
- Les hypothèses de coût et de revenu sont simplifiées : elles ne tiennent pas compte de la marge réelle ni de la valeur à long terme d'un client.
