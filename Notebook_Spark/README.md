# README — TP OverFast Spark

## Sujet
- Traitement avec Apache Spark de données Overwatch (héros) stockées dans Cassandra, afin d’illustrer l’ingestion depuis Cassandra, les transformations (filter, select, orderBy), la gestion des partitions, l’évaluation paresseuse (lazy evaluation) et la supervision via la Spark UI.

## Données
- Source: keyspace Cassandra construit au TP précédent.
- Thème: caractéristiques des héros d’Overwatch.
- Schéma (extrait) de la table: 
  - hero_key (text, PRIMARY KEY)
  - name (text)
  - role (text)
  - subrole (text)
  - age (int)
  - location (text)
  - health (int)
  - armor (int)
  - shields (int)
  - total_hp (int)

## Keyspace et table
- Keyspace: overwatch
- Table: heroes

## Question métier
- Identifier les héros les plus « résistants » (HP totaux) par catégorie (ex.: Tank) afin d’orienter la composition d’équipe et les choix stratégiques.

## Traitements réalisés
- Connexion et lecture des données Cassandra dans un DataFrame Spark (spark-cassandra-connector).
- Sélections et filtrages par catégorie (ex.: Tank) et colonnes d’intérêt (hero_key, categorie/role, total_hp).
- Tri décroissant sur total_hp pour établir les tops.
- Expérimentation du partitionnement: observation du nombre initial de partitions puis réorganisation par repartition(8).
- Illustration de la lazy evaluation: enchaînement de transformations puis déclenchement via l’action show().
- Supervision via Spark UI (http://localhost:4040): lecture des Jobs, Stages et Tasks pendant l’exécution.

## Principales observations
- Les héros Tank les plus hauts en HP totaux observés dans l’exécution du notebook incluent notamment: roadhog (750), wrecking-ball (725), mauga (725), reinhardt (700), suivi d’un groupe à 650–625–600–550–525.
- Le repartitionnement (ex.: 4 → 8) modifie le parallélisme potentiel des tâches sans changer les résultats métiers.
- La lazy evaluation permet à Spark d’optimiser le plan d’exécution avant de lancer les calculs; le DAG, les Stages et les Tasks sont visibles et traçables dans la Spark UI.

Notes
- Le cluster Cassandra utilisé a été configuré au TP précédent (RF=3, tests ONE/QUORUM/ALL). Ce TP se concentre sur l’exploitation de ces données via Spark.