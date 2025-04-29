SELECT ROUND(SUM("Average_Query_Age")/60) AS "ROUND(SUM(""Average_Query_Age"")/60)" 
FROM (SELECT 
  AVG("Query_age") AS "Average_Query_Age"
FROM public."Key-insight-data-2"
) AS virtual_table 
 LIMIT 50000;
