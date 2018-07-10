|Data | HackerNews |
|---------|-------------|
|Author | Kenneth Chen, PhD|
|Platform | GCP BigQuery |
|Date | July 9, 2018 |

# Summary

# Query commands
For standard SQL in BigQuery, use 
```
#standardSQL
SELECT
FROM
```

For SQL query from CLI, don't use any of the hash comment
```
# From the CLI window
bq query --use_legacy_sql=false 'SELECT FROM `table`'

# In jupyter notebook
!bq query --use_legacy_sql=false 'SELECT FROM `table`'

# To return 1 millions rows if necessary 
bq query --use_legacy_sql=false --max_rows=1000000 'SELECT FROM `table`' 

# To save the result in csv file
bq query --use_legacy_sql=false --max_rows=1000000 --format=csv 'SELECT FROM `table`' > myfile.csv
```


# Number of Rows
```
#standardSQL
SELECT COUNT(*)
FROM `bigquery-public-data.hacker_news.full` 
```

```
+------------+
| Total_News |
+------------+
|   17108307 |
+------------+
```

# Types of Texts
#### Standard SQL Format  
```
#standardSQL  
SELECT type, COUNT(*)  
FROM `bigquery-public-data.hacker_news.full`   
GROUP BY type  

#### CLI Format  

bq query --use_legacy_sql=false 'SELECT type, COUNT(*) as total_count FROM `bigquery-public-data.hacker_news.full` group by type'

#### Result 

+---------+-------------+
|  type   | total_count |
+---------+-------------+
| comment |    14136753 |
| story   |     2947287 |
| job     |       10620 |
| pollopt |       11909 |
| poll    |        1738 |
+---------+-------------+
```
There are 5 types of texts.  

# Number of Users
#### Standard SQL Format  
```
#standardSQL
SELECT COUNT(DISTINCT t1.by)
FROM `bigquery-public-data.hacker_news.full` as t1
```

#### CLI Format  
```
bq query --use_legacy_sql=false 'SELECT COUNT(DISTINCT t1.by) as Number_of_Users FROM `bigquery-public-data.hacker_news.full` t1'
```

```
+-----------------+
| Number_of_Users |
+-----------------+
|          524637 |
+-----------------+
```

```
#standardSQL
SELECT t1.by as User, COUNT(*) as total_post
FROM `bigquery-public-data.hacker_news.full` as t1 
GROUP BY t1.by
ORDER BY total_post DESC

# CLI format

bq query --use_legacy_sql=false 'SELECT t1.by as User, COUNT(*) as total_post FROM `bigquery-public-data.hacker_news.full` as t1 GROUP BY t1.by ORDER BY total_post DESC LIMIT 10'

# Result

+--------------+------------+
|     User     | total_post |
+--------------+------------+
|              |     541464 |
| tptacek      |      45249 |
| jacquesm     |      34156 |
| dragonwriter |      24017 |
| rbanffy      |      23957 |
| dang         |      21636 |
| DanBC        |      20514 |
| pjmlp        |      19083 |
| icebraining  |      18056 |
| mikeash      |      17968 |
+--------------+------------+
```
The user named 'tptacek' commented 45,249 posts, which is the highest posts in our data analysis. 

Since there are empty rows for **text** column, I checked total number of comments posted by each user. 

```
#standardSQL
SELECT t1.by as users, COUNT(*) as total_comments
FROM `bigquery-public-data.hacker_news.full` as t1 
where t1.text <> ''
GROUP BY t1.by
ORDER BY total_comments DESC

bq query --use_legacy_sql=false 'SELECT t1.by as users, COUNT(*) as total_comments FROM `bigquery-public-data.hacker_news.full` as t1 WHERE t1.text <> "" GROUP BY t1.by ORDER BY total_comments DESC LIMIT 10'

+--------------+----------------+
|    users     | total_comments |
+--------------+----------------+
| tptacek      |          44879 |
| jacquesm     |          32463 |
| dragonwriter |          24013 |
| dang         |          21474 |
| DanBC        |          19281 |
| pjmlp        |          18704 |
| icebraining  |          18025 |
| mikeash      |          17962 |
| coldtea      |          17662 |
| rayiner      |          16043 |
+--------------+----------------+
```
I found the top most commenter **tptacek** posts reduced from 45,249 to 44,879, accounting for 370 deficits in empty row. To make sure if each user has a fixed empty rows, I also checked the second most commenter **jacquesm** whose posts decreased from 34,156 to 32,463 which gave me 1,693 empty text rows. It appears many different users had different amount of empty rows for some reasons. 

# Summary
There are 17,108,307 news and 524,637 users on HackerNews. The user named 'tptacek' commented 45,249 posts, which is the highest posts in our data analysis.

**Note**  
Since the user column was labeled as 'by', to avoid the clash, I named the table as t1 and called the user number by t1.by.  
In text column, there are empty rows, not **NULL** row. In order to remove those rows in my counting mode, I used t1.text <> "".



