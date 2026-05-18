# Big Data & Open Data — supports de formation

Dépôt des **notebooks de TP**, des **sources de données** (`Docs/sources_data.md`) et des **dépendances Python** (`requirements.txt`). Les jeux de données volumineux et les supports de présentation restent en local (voir ci-dessous).

**Documentation d’environnement** : les procédures ci-dessous sont rédigées **en priorité pour Windows 10 / 11** (PowerShell ou Invite de commandes). Les équivalents **macOS** et **Linux** sont indiqués à côté ou dans des blocs séparés.

## Contenu versionné

| Élément | Rôle |
|---------|------|
| `TP/jour1/`, `TP/jour2/` | Notebooks **énoncés** (modules 1 à 5) |
| `Docs/sources_data.md` | Liens et jeux de données par TP |
| `requirements.txt` | Dépendances Python du projet |

## À préparer en local (hors dépôt Git)

| Élément | Rôle |
|---------|------|
| `TP_corrige/` | Notebooks **corrigés** (à récupérer à la fin du cours) |
| `donnees/` | CSV / Parquet téléchargés (voir `Docs/sources_data.md`) |

Créer le dossier `donnees/` à la racine du projet avant les TP qui lisent des fichiers locaux.

### Liste des notebooks

| Fichier | TP |
|---------|-----|
| `TP/jour1/TP_1_1_Comparaison_donnees_educatifs.ipynb` | 1.1 |
| `TP/jour1/TP_2_1_PySpark_Annuaire.ipynb` | 2.1 |
| `TP/jour1/TP_2_2_HDFS_Hadoop.ipynb` | 2.2 (guide HDFS / Hadoop sur la VM) |
| `TP/jour2/TP_3_1_PostgreSQL_Annuaire.ipynb` | 3.1 |
| `TP/jour2/TP_3_2_DataLake_Medallion.ipynb` | 3.2 |
| `TP/jour2/TP_4_1_EDA_IVAL.ipynb` | 4.1 |
| `TP/jour2/TP_4_2_Dashboard_IVAL.ipynb` | 4.2 (checklist Power BI / Tableau) |
| `TP/jour2/TP_5_1_Strategie_Rectorat.ipynb` | 5.1 |

Les URL et jeux de données sont centralisés dans `Docs/sources_data.md`.

---

## Mode opératoire — avant la session

### 1. Versions recommandées

| Composant | Détail |
|-----------|--------|
| **Python** | 3.10 ou 3.11, **64 bits** (obligatoire pour PySpark). Windows : installer depuis [python.org](https://www.python.org/downloads/) en cochant **« Add python.exe to PATH »**, ou utiliser le gestionnaire de paquets de votre organisme. |
| **Java** | **11 ou 17** (obligatoire pour PySpark en local). Windows : installer un JDK (ex. [Eclipse Temurin](https://adoptium.net/) ou Oracle JDK), puis définir `JAVA_HOME` (voir plus bas). |

**Option avancée (Windows)** : exécuter Python / Jupyter dans **WSL2** (Ubuntu) revient alors aux commandes **Linux** décrites plus bas.
*/!\ Nous utiliserons la dernière version **Python 3.14** pour la formation*

### 2. Installer les dépendances Python

Placez le dépôt dans un chemin **sans espaces** si possible (ex. `C:\dev\Big_data_open_data`) pour limiter les soucis avec certains outils.

**Windows (PowerShell ou CMD)** — à la racine du dépôt, avec le **Python déjà installé sur la VM** (pas de venv) :

```bat
cd C:\chemin\vers\Big_data_open_data
python -m pip install -U pip
python -m pip install -r requirements.txt
```

Si la commande `python` n’est pas reconnue, essayez le lanceur Windows : `py -m pip install -U pip` puis `py -m pip install -r requirements.txt`.

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

**Windows** — sous Windows, la commande `jupyter` n’est souvent **pas dans le PATH** même après `pip install`. Utiliser le module Python :

```bat
cd C:\chemin\vers\Big_data_open_data
python -m jupyter lab
```

Si `python` n’est pas reconnu : `py -m jupyter lab`.

En cas d’erreur « No module named jupyter » : relancer l’installation des dépendances (`python -m pip install -r requirements.txt`), puis réessayer.

**macOS / Linux** :

```bash
cd /chemin/vers/Big_data_open_data
source .venv/bin/activate
jupyter lab
```

Ouvrir les notebooks dans `TP\jour1\` ou `TP\jour2\`. Si un notebook ne trouve pas `donnees\`, vérifier dans Jupyter le dossier d’origine du serveur (*File → Open from Path* ou redémarrer après un `cd` correct vers la racine).

---

## Vérifier l’installation — Spark / PySpark

Dans un terminal :

**Windows** (Python système de la VM) :

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

Le TP 2.2 repose sur une **démonstration HDFS** avec **Hadoop installé sur la VM Windows** de formation (pas de Docker). Le notebook `TP/jour1/TP_2_2_HDFS_Hadoop.ipynb` détaille les étapes.

### Prérequis (VM Windows)

- **Hadoop** et le client **`hdfs`** déjà installés et présents dans le `PATH` (fourni sur la VM).
- Variable **`HADOOP_HOME`** pointant vers l’installation Hadoop (ex. `C:\hadoop`).
- **Java** 11 ou 17 et **`JAVA_HOME`** configurés (voir section PySpark ci-dessus).
- Fichier CSV des effectifs VP/BTS dans `donnees\` (voir `Docs/sources_data.md`, section TP 2.2).

### Vérifier et démarrer Hadoop

Dans **PowerShell** ou **CMD** :

```powershell
hdfs dfs -ls /
jps
```

`jps` doit afficher au minimum **NameNode** et **DataNode**. Si `hdfs dfs` échoue (NameNode injoignable), démarrer HDFS (adapter le chemin si besoin) :

```powershell
%HADOOP_HOME%\sbin\start-dfs.cmd
%HADOOP_HOME%\sbin\start-yarn.cmd
```

Interface web du NameNode : **http://localhost:9870**

### Commandes utiles (TP 2.2)

Depuis la racine du dépôt, en adaptant le nom d’utilisateur Windows (`formation`) et le fichier CSV :

```powershell
hdfs dfs -mkdir -p /user/formation/data
hdfs dfs -put .\donnees\effectifs_vp_bts.csv /user/formation/data/
hdfs dfs -ls /user/formation/data
hdfs fsck /user/formation/data/effectifs_vp_bts.csv -files -blocks
```

### Dépannage Windows

| Problème | Piste |
|----------|--------|
| `'hdfs' n'est pas reconnu` | Vérifier `HADOOP_HOME` et que `%HADOOP_HOME%\bin` est dans le `PATH` ; rouvrir le terminal. |
| Erreur de connexion au NameNode | Lancer `start-dfs.cmd` (voir ci-dessus), attendre quelques secondes, retester `hdfs dfs -ls /`. |
| `put` : fichier introuvable | Vérifier le chemin Windows du CSV dans `donnees\` (guillemets si espaces dans le chemin). |
| Page **9870** inaccessible | Vérifier que le NameNode tourne (`jps`). |

Arrêt du cluster (fin de session) : `%HADOOP_HOME%\sbin\stop-dfs.cmd` et `stop-yarn.cmd`.

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
