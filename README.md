# Segmentation et score d'appétence : qui cibler, et combien ça rapporte

> Segmentation d'une base de 2 240 clients d'une enseigne de distribution, modèle de prédiction de la réponse aux campagnes et simulation du retour sur investissement d'un envoi ciblé.

**Outils :** Python (pandas, scikit-learn)

---

## 1. Contexte

Une enseigne de distribution alimentaire vend en magasin, sur son site web et par catalogue. Elle a mené six campagnes marketing, envoyées à l'ensemble de sa base sans ciblage, avec des taux d'acceptation compris entre 1 et 15 % environ.

La direction marketing veut mieux connaître ses clients et savoir si elle peut obtenir autant de réponses en contactant moins de monde.

La note de cadrage complète est disponible dans [`docs/brief_cadrage.md`](docs/brief_cadrage.md).

## 2. Questions traitées

1. Quels profils d'achat se dégagent, selon les canaux, les catégories de produits et le foyer ?
2. Peut-on regrouper la base en quatre à cinq segments compréhensibles et actionnables ?
3. Peut-on estimer, pour chaque client, sa probabilité de répondre à la prochaine campagne ?
4. Quel résultat financier obtient-on en ne ciblant que les clients les mieux scorés ?

## 3. Données

| Élément | Détail |
|---|---|
| Jeu de données | *Customer Personality Analysis* — Kaggle |
| Lien | https://www.kaggle.com/datasets/imakash3011/customer-personality-analysis |
| Fichier | `marketing_campaign.csv` (séparateur : tabulation) |
| Volume | 2 240 clients, 29 variables |
| Variables | Profil (âge, éducation, situation familiale, revenu, enfants), dépenses sur deux ans par catégorie, achats par canal, réponses à six campagnes |
| Hypothèses économiques | Coût d'un contact : 3 u.m. · Revenu par réponse : 11 u.m. (colonnes `Z_CostContact` et `Z_Revenue`) |

Les données ne sont pas incluses dans ce dépôt. Pour reproduire l'analyse, téléchargez le fichier sur Kaggle et placez-le dans le dossier `data/`.

## 4. Méthode

| Étape | Ce qui est fait | Notebook |
|---|---|---|
| Préparation | Revenus manquants, valeurs aberrantes, recodage, création de variables (âge, ancienneté, foyer, dépense totale, parts par canal et par catégorie, part des achats en promotion) | `01_preparation` |
| Comportements | Profils d'achat par canal, catégorie, revenu et composition du foyer | `02_comportements` |
| Segmentation | Standardisation, k-means, choix du nombre de segments (méthode du coude, score de silhouette), portrait de chaque segment | `03_segmentation` |
| Score d'appétence | Régression logistique sur la réponse à la dernière campagne, AUC, variables déterminantes, courbe de gains | `04_score` |
| Performance et ROI | Taux d'acceptation par campagne et par segment, simulation de ciblage par déciles, analyse de sensibilité | `05_roi` |

## 5. Résultats

| Indicateur | Valeur |
|---|---|
| Clients analysés après préparation | 2 232 (8 lignes écartées sur 2 240) |
| Taux de réponse à la dernière campagne | 14,9 % |
| Nombre de segments retenus | 4 (méthode du coude ; silhouette de 0,24 : segments qui se recouvrent en partie, retenus pour leur lisibilité métier) |
| Qualité du score (AUC) | 0,90 sur des clients non vus à l'apprentissage, pour un taux de réponse de base de 14,9 % (un score aléatoire obtiendrait 0,50) |
| Part des répondants captée en ciblant 30 % de la base | 85 % (48 % avec les 10 % les mieux scorés) |
| Résultat d'un envoi à toute la base | −3 032 u.m. (ROI −45 %) |
| Résultat d'un envoi ciblé sur le top 20 % (optimum) | +1 203 u.m. (ROI +90 %), avec 70 % des répondants et 80 % de contacts en moins |
| Méthode de simulation | Scores hors échantillon (validation croisée en 5 parties) : aucun client n'est noté par un modèle qui l'a vu à l'apprentissage |

### Segments

| Segment | Part de la base | Part du chiffre d'affaires | Taux de réponse | Portrait |
|---|---|---|---|---|
| Clients premium | 24 % | 53 % | 26 % | Foyers aisés presque sans enfant, plus forte dépense, peu sensibles aux promotions, adeptes du catalogue |
| Réguliers amateurs de vin | 27 % | 37 % | 13 % | Revenus intermédiaires, achats fréquents sur tous les canaux, le vin représente 58 % du panier |
| Familles chasseuses de promotions | 21 % | 7 % | 16 % | Foyers avec près de deux enfants, un achat sur deux en promotion, très présents sur le site |
| Petits budgets occasionnels | 28 % | 3 % | 7 % | Clients plus jeunes aux revenus modestes, achats rares et faibles |

## 6. Recommandations

1. **Arrêter les envois à toute la base.** Avec un coût de 3 u.m. par contact et un revenu de 11 u.m. par réponse, une campagne n'est rentable qu'au-delà de 27 % de réponse. Aucune des six campagnes n'a atteint ce seuil : un envoi à toute la base fait perdre entre 3 000 et 6 400 u.m.
2. **Cibler les 20 % de clients les mieux scorés.** Ce ciblage transforme une perte de 3 032 u.m. en un gain de 1 203 u.m., soit 4 235 u.m. d'écart, en conservant 70 % des répondants et avec 80 % de contacts en moins. L'analyse de sensibilité confirme qu'il reste rentable dans les neuf combinaisons de coût et de revenu testées, avec une part optimale comprise entre 10 et 30 % de la base.
3. **Adapter le message au segment.** Le score décide qui contacter, la segmentation guide ce que l'on dit : offres exclusives et catalogue pour les clients premium, sélections de vins pour les réguliers amateurs de vin, promotions familiales pour les familles chasseuses de promotions. Les petits budgets occasionnels relèvent de canaux à coût quasi nul, comme l'e-mail ou le site.

## 7. Limites

- Les données datent de 2012-2014 et proviennent d'un jeu public : le projet illustre une méthode, il ne décrit pas un marché actuel.
- Le score est entraîné sur la dernière campagne ; il suppose que la suivante sera comparable.
- Les hypothèses de coût et de revenu sont simplifiées et ne tiennent compte ni de la marge réelle ni de la valeur à long terme d'un client.

## 8. Structure du dépôt

```
customer-segmentation-campaign-scoring/
├── README.md
├── requirements.txt
├── docs/
│   └── brief_cadrage.md          note de cadrage du projet
├── data/
│   └── marketing_campaign.csv    à télécharger (non versionné)
├── notebooks/
│   ├── 01_preparation.ipynb      préparation des données
│   ├── 02_comportements.ipynb    analyse des comportements d'achat
│   ├── 03_segmentation.ipynb     segmentation en 4 segments
│   ├── 04_score.ipynb            score d'appétence
│   └── 05_roi.ipynb              performance des campagnes et ROI
└── outputs/                      fichiers générés par les notebooks (non versionnés)
```

## 9. Reproduire l'analyse

```bash
git clone https://github.com/niyadanielleO/customer-segmentation-campaign-scoring.git
cd customer-segmentation-campaign-scoring
python3 -m venv .venv
source .venv/bin/activate
pip install -r requirements.txt
jupyter notebook
```

Placer ensuite `marketing_campaign.csv` dans `data/` et exécuter les notebooks dans l'ordre.

---

**Niya C. Danielle Ouattara** · [LinkedIn](https://www.linkedin.com/in/ndanielleouattara/) · [Autres projets](https://github.com/niyadanielleO)
