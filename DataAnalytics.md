|Data | HackerNews |
|---------|-------------|
|Author | Kenneth Chen, PhD|
|Platform | GCP BigQuery |
|Date | July 9, 2018 |

# Summary



# 
#### Standard SQL Format  
`
#standardSQL
SELECT type, COUNT(*)
FROM `bigquery-public-data.hacker_news.full` 
GROUP BY type
`

#### CLI SQL Format  
`
bq query --use_legacy_sql=false 'SELECT type, COUNT(*) as total_count FROM `bigquery-public-data.hacker_news.full` group by type'
`


|  type   | total_count |
|---------|-------------|
| comment |    14136753 |
| story   |     2947287 |
| job     |       10620 |
| pollopt |       11909 |
| poll    |        1738 |
|---------|-------------|
