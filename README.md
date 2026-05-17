# Big Data & Open Data — supports de formation

Dépôt des **notebooks de TP**, des **sources de données** (`Docs/sources_data.md`) et des **dépendances Python** (`requirements.txt`). Les jeux de données volumineux et les supports de présentation restent en local (voir ci-dessous).

**Documentation d’environnement** : les procédures ci-dessous sont rédigées **en priorité pour Windows 10 / 11** (PowerShell ou Invite de commandes). Les équivalents **macOS** et **Linux** sont indiqués à côté ou dans des blocs séparés.

## Contenu versionné

| Élément | Rôle |
|---------|------|
| `TP/jour1/`, `TP/jour2/` | Notebooks **énoncés** (modules 1 à 5) |
| `TP_corrige/jour1/`, `TP_corrige/jour2/` | Notebooks **corrigés** (même découpage) |
| `Docs/sources_data.md` | Liens et jeux de données par TP |
| `requirements.txt` | Dépendances Python du projet |

## À préparer en local (hors dépôt Git)

| Élément | Rôle |
|---------|------|
| `donnees/` | CSV / Parquet téléchargés (voir `Docs/sources_data.md`) |
| `Diaporama_cours/` | Support de présentation (diaporama) |
| `_scripts/` | Script utilitaire `write_notebooks.py` (régénération des notebooks si besoin) |

Créer le dossier `donnees/` à la racine du projet avant les TP qui lisent des fichiers locaux.

### Liste des notebooks

| Fichier | TP |
|---------|-----|
| `TP/jour1/TP_1_1_Comparaison_donnees_educatifs.ipynb` | 1.1 |
| `TP/jour1/TP_2_1_PySpark_Annuaire.ipynb` | 2.1 |
| `TP/jour1/TP_2_2_HDFS_Hadoop.ipynb` | 2.2 (guide HDFS / Docker) |
| `TP/jour2/TP_3_1_PostgreSQL_Annuaire.ipynb` | 3.1 |
| `TP/jour2/TP_3_2_DataLake_Medallion.ipynb` | 3.2 |
| `TP/jour2/TP_4_1_EDA_IVAL.ipynb` | 4.1 |
| `TP/jour2/TP_4_2_Dashboard_IVAL.ipynb` | 4.2 (checklist Power BI / Tableau) |
| `TP/jour2/TP_5_1_Strategie_Rectorat.ipynb` | 5.1 |

Les versions corrigées sont dans `TP_corrige/` (mêmes noms de fichiers). Les URL et jeux de données sont centralisés dans `Docs/sources_data.md`.

---

## Mode opératoire — avant la session

### 1. Versions recommandées

| Composant | Détail |
|-----------|--------|
| **Python** | 3.10 ou 3.11, **64 bits** (obligatoire pour PySpark). Windows : installer depuis [python.org](https://www.python.org/downloads/) en cochant **« Add python.exe to PATH »**, ou utiliser le gestionnaire de paquets de votre organisme. |
| **Java** | **11 ou 17** (obligatoire pour PySpark en local). Windows : installer un JDK (ex. [Eclipse Temurin](https://adoptium.net/) ou Oracle JDK), puis définir `JAVA_HOME` (voir plus bas). |

**Option avancée (Windows)** : exécuter Python / Jupyter dans **WSL2** (Ubuntu) revient alors aux commandes **Linux** décrites plus bas.

### 2. Créer l’environnement virtuel

Placez le dépôt dans un chemin **sans espaces** si possible (ex. `C:\dev\Big_data_open_data`) pour limiter les soucis avec certains outils.

**Windows (PowerShell ou CMD)** — à la racine du dépôt :

```powershell
cd C:\chemin\vers\Big_data_open_data
py -3.11 -m venv .venv
.\.venv\Scripts\Activate.ps1
```

Si l’activation PowerShell est bloquée par la stratégie d’exécution :

```powershell
Set-ExecutionPolicy -Scope CurrentUser RemoteSigned
```

Ou utilisez **CMD** :

```bat
cd C:\chemin\vers\Big_data_open_data
py -3.11 -m venv .venv
.venv\Scripts\activate.bat
```

Puis dans les deux cas :

```bat
python -m pip install -U pip
pip install -r requirements.txt
```

**macOS / Linux** (bash ou zsh) :

```bash
cd /chemin/vers/Big_data_open_data
python3 -m venv .venv
source .venv/bin/activate
pip install -U pip
pip install -r requirements.txt
```

### 3. Télécharger les jeux de données

Suivre `Docs/sources_data.md`. Au minimum : **Annuaire** et **IVAL GT** en CSV dans le dossier `donnees/` (à créer à la racine). Sous Windows, le plus simple est souvent le **navigateur** (enregistrer le fichier puis le déplacer dans `donnees\`).

Exemples de commandes (macOS / Linux) :

```bash
mkdir -p donnees
curl -L -o donnees/fr-en-annuaire-education.csv \
  "https://data.education.gouv.fr/api/explore/v2.1/catalog/datasets/fr-en-annuaire-education/exports/csv"
curl -L -o donnees/fr-en-indicateurs-de-resultat-des-lycees-gt_v2.csv \
  "https://data.education.gouv.fr/api/explore/v2.1/catalog/datasets/fr-en-indicateurs-de-resultat-des-lycees-gt_v2/exports/csv"
```

### 4. Lancer Jupyter

Le serveur Jupyter doit être lancé de façon que la **racine du dépôt** soit le répertoire de travail (les notebooks cherchent le dossier `donnees\` à partir de cette racine).

**Windows** :

```bat
cd C:\chemin\vers\Big_data_open_data
.\.venv\Scripts\activate.bat
jupyter lab
```

**macOS / Linux** :

```bash
cd /chemin/vers/Big_data_open_data
source .venv/bin/activate
jupyter lab
```

Ouvrir les notebooks dans `TP\jour1\`, `TP\jour2\` (ou `TP_corrige\` pour les corrigés). Si un notebook ne trouve pas `donnees\`, vérifier dans Jupyter le dossier d’origine du serveur (*File → Open from Path* ou redémarrer après un `cd` correct vers la racine).

---

## Vérifier l’installation — Spark / PySpark

Dans un terminal avec le **venv activé** :

**Windows** :

```bat
python -c "import pyspark; print('PySpark', pyspark.__version__)"
python -c "from pyspark.sql import SparkSession; SparkSession.builder.master('local[1]').appName('test').getOrCreate().stop(); print('SparkSession OK')"
```

**macOS / Linux** : mêmes commandes en remplaçant `python` par `python3` si nécessaire.

### Java et variable `JAVA_HOME`

| Système | Action |
|---------|--------|
| **Windows** | Après installation du JDK, *Paramètres → Système → Informations système → Paramètres système avancés → Variables d’environnement* : créer ou modifier `JAVA_HOME` = dossier du JDK (ex. `C:\Program Files\Eclipse Adoptium\jdk-17.x.x-hotspot`), et ajouter `%JAVA_HOME%\bin` au `Path`. Fermer et rouvrir le terminal. Vérifier : `java -version` (JVM 11 ou 17). |
| **macOS (Homebrew)** | `brew install openjdk@17` puis dans `~/.zshrc` : `export JAVA_HOME=$(/usr/libexec/java_home -v 17)` ou, sur Apple Silicon : `export JAVA_HOME="/opt/homebrew/opt/openjdk@17/libexec/openjdk.jdk/Contents/Home"`. |
| **Linux (Debian/Ubuntu)** | Ex. `sudo apt install openjdk-17-jdk` puis `export JAVA_HOME=/usr/lib/jvm/java-17-openjdk-amd64` (adapter selon `update-java-alternatives -l`). |

Les notebooks PySpark utilisent `master("local[*]")` : aucun cluster à installer pour les TP en local.

---

## Hadoop / HDFS (TP 2.2)

Le TP 2.2 repose sur une **démonstration** : mini-cluster Hadoop (souvent via **Docker**). Il n’est pas nécessaire d’installer Hadoop sur la machine hôte si vous utilisez une image Docker fournie par le formateur.

### Prérequis

- **Windows** : [Docker Desktop pour Windows](https://docs.docker.com/desktop/install/windows-install/) (WSL2 recommandé en backend ; suivre l’assistant d’installation Docker).
- **macOS** : Docker Desktop.
- **Linux** : Docker Engine + plugin Compose, ou Docker Desktop selon votre environnement.

### Vérifier Docker

**Windows / macOS / Linux** (même syntaxe) :

```bash
docker version
docker run --rm hello-world
```

### Lancer un environnement HDFS de test (exemple générique)

Les images exactes varient selon le formateur. Le notebook `TP/jour1/TP_2_2_HDFS_Hadoop.ipynb` rappelle les commandes types :

- copier un fichier local vers HDFS : `hdfs dfs -put ...`
- lister : `hdfs dfs -ls /`
- intégrité des blocs : `hdfs fsck /chemin/fichier -files -blocks`
- interface **NameNode** (souvent port **9870** selon l’image).

**Donnée conseillée** : jeu des effectifs VP/BTS (voir `Docs/sources_data.md`, section TP 2.2), placé dans `donnees\` puis monté ou copié dans le conteneur selon les consignes du formateur.

### Si Docker n’est pas disponible

Traiter le TP 2.2 en **salle** sur les postes prévus, ou en lecture du notebook (étapes et captures) sans exécution.

---

## PostgreSQL (TP 3.1)

Le TP 3.1 utilise une base **PostgreSQL** pour charger l’annuaire et exécuter des requêtes SQL (index, `EXPLAIN ANALYZE`). Le détail est dans `TP/jour2/TP_3_1_PostgreSQL_Annuaire.ipynb`.

### Option A — Docker (recommandé)

**Windows / macOS / Linux** :

```bash
docker run -d --name pg-tp -e POSTGRES_USER=tp -e POSTGRES_PASSWORD=tptp -e POSTGRES_DB=education -p 5432:5432 postgres:16
```

Vérification : `docker ps`. Connexion (exemple) : `psql postgresql://tp:tptp@localhost:5432/education` ou **pgAdmin 4** / **DBeaver** avec les mêmes paramètres hôte `localhost`, port `5432`, base `education`.

### Option B — PostgreSQL installé localement

Installer **PostgreSQL** (installateur Windows depuis postgresql.org, paquets type `apt` / `dnf` sous Linux, Postgres.app ou Homebrew sous macOS), créer une base `education` et un utilisateur, puis adapter l’URL dans le notebook (`DATABASE_URL`).

**Dépendances Python** : `psycopg2-binary` et `sqlalchemy` (déjà listés dans `requirements.txt`).

---

## Power BI / Tableau (TP 4.2)

Ce TP est principalement réalisé **dans l’outil BI** (souvent **Power BI Desktop** sous Windows). Le notebook associé donne la checklist et les liens ; les données sources sont les CSV IVAL dans `donnees\` (éventuellement le Parquet produit au TP 3.2).

---

## Fichier utile

- **Sources et URL** : `Docs/sources_data.md`

---

## Licence des données

Les jeux open data cités dans `Docs/sources_data.md` sont en général sous **Licence Ouverte 2.0** : mentionner la source et la date de téléchargement dans les restitutions.
