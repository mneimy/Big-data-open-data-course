# Sources de données — Big Data & Open Data (Éducation nationale)

Ce fichier recense les jeux de données et liens utiles pour les travaux pratiques. Les colonnes et URL peuvent évoluer : vérifier la fiche du jeu sur le portail avant chaque session.

### Environnement (Windows, macOS, Linux)

- **Windows** : enregistrer les exports depuis le navigateur dans le dossier `donnees\` du dépôt, ou utiliser les commandes décrites dans `donnees/README.md` (PowerShell `Invoke-WebRequest` ou `curl.exe`). Les notebooks détectent la racine du projet automatiquement ; lancer **Jupyter** depuis la racine du dépôt (voir `README.md` à la racine).
- **macOS / Linux** : mêmes URL ; exemples `curl` et activation du venv dans `README.md` et `donnees/README.md`.

---

## Portails de référence

| Portail | URL |
|--------|-----|
| Open Data du ministère (Éducation nationale) | https://data.education.gouv.fr |
| data.gouv.fr (interministériel) | https://www.data.gouv.fr |
| Thématique « Données éducation » sur data.gouv.fr | https://www.data.gouv.fr/pages/donnees_education/ |
| Catalogue des API publiques de l'État | https://api.gouv.fr |
| Enseignement supérieur et recherche | https://data.esr.gouv.fr |
| IVAL / IVAC (page ministérielle) | https://www.education.gouv.fr/les-indicateurs-de-resultats-des-colleges-et-des-lycees-377729 |

---

## TP 1.2 — API data.gouv.fr & data.education.gouv.fr

Notebook : `TP/jour1/TP_1_2_API_data_gouv_education.ipynb` (corrigé dans `TP_corrige/jour1/`).

| Usage | URL / endpoint |
|-------|----------------|
| Recherche jeux (API v1) | `https://www.data.gouv.fr/api/1/datasets/?q=…` |
| Fiche jeu | `https://www.data.gouv.fr/api/1/datasets/{id}/` |
| Ressources (API v2) | `https://www.data.gouv.fr/api/2/datasets/{id}/resources/` |
| Enregistrements annuaire (ODS v2.1) | `https://data.education.gouv.fr/api/explore/v2.1/catalog/datasets/fr-en-annuaire-education/records` |
| Export CSV annuaire | `https://data.education.gouv.fr/api/explore/v2.1/catalog/datasets/fr-en-annuaire-education/exports/csv` |
| Doc API data.gouv.fr | https://www.data.gouv.fr/fr/dataservices/api/ |

**MCP Cursor** : les outils `search_datasets`, `get_dataset_info`, `list_dataset_resources` et `query_resource_data` sur le serveur `datagouv` reproduisent une partie de ce parcours depuis l’assistant.

---

## TP 1.1 — Comparaison de jeux de données éducatifs

### Jeu principal : Annuaire de l'éducation

- **data.gouv.fr** : https://www.data.gouv.fr/datasets/annuaire-de-leducation/
- **data.education.gouv.fr** : https://data.education.gouv.fr/explore/dataset/fr-en-annuaire-education/
- **Source** : DEPP (données ONISEP et Ramsese)

### Jeux alternatifs

**Indicateurs de valeur ajoutée des lycées (IVAL)**

- Voie générale et technologique : https://www.data.gouv.fr/datasets/indicateurs-de-valeur-ajoutee-des-lycees-denseignement-general-et-technologique-2/
- Voie professionnelle : https://www.data.gouv.fr/datasets/indicateurs-de-valeur-ajoutee-des-lycees-denseignement-professionnel/

**Effectifs d'élèves par école**

- https://www.data.gouv.fr/datasets/effectifs-deleves-par-niveau-et-nombre-de-classes-par-ecole-date-dobservation-au-debut-du-mois-doctobre-chaque-annee/

---

## TP 2.1 — Exploration des établissements avec PySpark

**Partie A — Annuaire** (~69 000 établissements, CSV ~40 Mo) : exploration et agrégations de base.

| Usage | URL |
|-------|-----|
| Page data.gouv.fr | https://www.data.gouv.fr/datasets/annuaire-de-leducation/ |
| Exploration data.education | https://data.education.gouv.fr/explore/dataset/fr-en-annuaire-education/ |
| **Export CSV direct (API v2.1)** | https://data.education.gouv.fr/api/explore/v2.1/catalog/datasets/fr-en-annuaire-education/exports/csv |

**Partie B — Effectifs VP/BTS** (~310 000 lignes, CSV ~63 Mo) : comparer Spark vs pandas sur un jeu plus volumineux (même jeu que le TP 2.2).

| Usage | URL |
|-------|-----|
| Page data.gouv.fr | https://www.data.gouv.fr/datasets/effectifs-des-eleves-en-voie-professionnelle-ou-bts-par-niveau-sexe-et-lycee-professionnel-date-dobservation-au-debut-du-mois-doctobre-chaque-annee/ |
| Exploration data.education | https://data.education.gouv.fr/explore/dataset/fr-en-lycee_pro-effectifs-niveau-sexe-mef/ |
| **Export CSV direct (API v2.1)** | https://data.education.gouv.fr/api/explore/v2.1/catalog/datasets/fr-en-lycee_pro-effectifs-niveau-sexe-mef/exports/csv |

---

## TP 2.2 — Démonstration HDFS / Hadoop

**Jeu recommandé** : effectifs en voie professionnelle ou BTS (fichier assez volumineux pour illustrer le découpage en blocs).

- https://www.data.gouv.fr/datasets/effectifs-des-eleves-en-voie-professionnelle-ou-bts-par-niveau-sexe-et-lycee-professionnel-date-dobservation-au-debut-du-mois-doctobre-chaque-annee/

**Alternative** : effectifs par école (national, souvent plus volumineux).

- https://data.education.gouv.fr/explore/dataset/fr-en-ecoles-effectifs-nb_classes/

---

## TP 3.1 — Base PostgreSQL (annuaire)

**Jeu** : Annuaire de l'éducation (**CSV** recommandé pour `COPY` / chargement via pandas ; JSON reste optionnel pour d'autres usages).

| Usage | URL |
|-------|-----|
| Page data.gouv.fr | https://www.data.gouv.fr/datasets/annuaire-de-leducation/ |
| **Export CSV direct (API v2.1)** | https://data.education.gouv.fr/api/explore/v2.1/catalog/datasets/fr-en-annuaire-education/exports/csv |
| Export JSON (API v2.1, optionnel) | https://data.education.gouv.fr/api/explore/v2.1/catalog/datasets/fr-en-annuaire-education/exports/json |
| API REST (alternative à l'import fichier) | https://www.data.gouv.fr/dataservices/annuaire-de-leducation-nationale/ |

---

## TP 3.2 — Data Lake éducatif (architecture Medallion)

**Jeu** : IVAL — voie générale et technologique.

| Usage | URL |
|-------|-----|
| data.gouv.fr | https://www.data.gouv.fr/datasets/indicateurs-de-valeur-ajoutee-des-lycees-denseignement-general-et-technologique-2/ |
| Exploration data.education | https://data.education.gouv.fr/explore/dataset/fr-en-indicateurs-de-resultat-des-lycees-gt_v2/ |
| **Export CSV direct (API v2.1)** | https://data.education.gouv.fr/api/explore/v2.1/catalog/datasets/fr-en-indicateurs-de-resultat-des-lycees-gt_v2/exports/csv |
| Guide méthodologique DEPP | https://www.education.gouv.fr/les-indicateurs-de-resultats-des-colleges-et-des-lycees-377729 |

---

## TP 4.1 — Analyse exploratoire (résultats baccalauréat / IVAL)

**Jeu principal** : IVAL voie GT.

- https://data.education.gouv.fr/explore/dataset/fr-en-indicateurs-de-resultat-des-lycees-gt_v2/
- https://www.data.gouv.fr/datasets/indicateurs-de-valeur-ajoutee-des-lycees-denseignement-general-et-technologique-2/
- **Export CSV** : https://data.education.gouv.fr/api/explore/v2.1/catalog/datasets/fr-en-indicateurs-de-resultat-des-lycees-gt_v2/exports/csv

**Enrichissement recommandé** : IPS des lycées.

- https://data.education.gouv.fr/explore/dataset/fr-en-ips-lycees-ap2023/
- **Export CSV** : https://data.education.gouv.fr/api/explore/v2.1/catalog/datasets/fr-en-ips-lycees-ap2023/exports/csv

---

## TP 4.2 — Tableau de bord IVAL (Power BI / Tableau)

**Jeux** : IVAL voie GT et voie pro.

| Jeu | Exploration | Export CSV (API v2.1) |
|-----|-------------|------------------------|
| IVAL GT | https://data.education.gouv.fr/explore/dataset/fr-en-indicateurs-de-resultat-des-lycees-gt_v2/ | `.../datasets/fr-en-indicateurs-de-resultat-des-lycees-gt_v2/exports/csv` |
| IVAL Pro | https://data.education.gouv.fr/explore/dataset/fr-en-indicateurs-de-resultat-des-lycees-pro_v2/ | `.../datasets/fr-en-indicateurs-de-resultat-des-lycees-pro_v2/exports/csv` |

**Cartographie (choroplèthe par académie)** : rechercher sur data.gouv.fr les contours des académies ou jeux géographiques INSEE adaptés au fond de carte.

---

## TP 5.1 — Stratégie data pour un rectorat (synthèse)

### Données ouvertes — éducation

| Thème | URL |
|-------|-----|
| Annuaire | https://www.data.gouv.fr/datasets/annuaire-de-leducation/ |
| IVAL / IVAC (page ministérielle) | https://www.education.gouv.fr/les-indicateurs-de-resultats-des-colleges-et-des-lycees-377729 |
| Portail data.education (accueil) | https://data.education.gouv.fr/pages/accueil/ |
| IPS écoles | https://www.data.gouv.fr/datasets/indices-de-position-sociale-dans-les-ecoles-a-partir-de-2022/ |
| IPS collèges | https://data.education.gouv.fr/explore/dataset/fr-en-ips-colleges-ap2023/ |
| IPS lycées | https://data.education.gouv.fr/explore/dataset/fr-en-ips-lycees-ap2023/ |

### Données ouvertes — croisements territoriaux / économie

| Source | URL |
|--------|-----|
| INSEE (recensement, projections) | https://www.insee.fr/fr/statistiques |
| Base Sirene | https://www.data.gouv.fr/datasets/base-sirene-des-entreprises-et-de-leurs-etablissements-siren-siret/ |
| Observatoire des territoires | https://www.observatoire-des-territoires.gouv.fr/ |

---

## Notes importantes

1. **Versions** : la DEPP met à jour les jeux chaque année (souvent entre février et avril). Toujours prendre la fiche la plus récente sur le portail.
2. **Licence** : en général **Licence Ouverte 2.0** (Etalab) pour les jeux DEPP — confirmer sur chaque fiche.
3. **Identifiants de jeux** : privilégier les datasets suffixés `_v2` ou `ap2023` / `ap2024` lorsqu’ils existent (méthodologies à jour).
4. **Formation** : télécharger les fichiers **en amont** et les placer dans `donnees\` (Windows) ou `donnees/` (macOS/Linux) — voir `README.md` et `donnees/README.md` à la racine du dépôt — pour limiter les aléas réseau en salle.

---

## Convention de noms de fichiers locaux (dossier `donnees/`)

| Fichier local suggéré | Jeu |
|----------------------|-----|
| `fr-en-annuaire-education.csv` | Annuaire (export API ou téléchargement manuel) |
| `fr-en-indicateurs-de-resultat-des-lycees-gt_v2.csv` | IVAL GT |
| `fr-en-ips-lycees-ap2023.csv` | IPS lycées (TP 4.1 optionnel) |
| `effectifs_vp_bts.csv` | Effectifs VP/BTS (TP 2.2, nom libre si renommé après téléchargement) |

Les notebooks supposent ces noms lorsque le fichier est présent dans le dossier `donnees` à la racine du dépôt (`donnees\` sous Windows) ; sinon ils indiquent comment télécharger via les URL ci-dessus.
