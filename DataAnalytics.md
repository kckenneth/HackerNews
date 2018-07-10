|Data | HackerNews |
|---------|-------------|
|Author | Kenneth Chen, PhD|
|Platform | GCP BigQuery |
|Date | July 9, 2018 |

# Summary

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

**Note**  
Since the user column was labeled as 'by', to avoid the clash, I named the table as t1 and called the user number by t1.by

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


# Summary
There are 17,108,307 news and 524,637 users on HackerNews. 

