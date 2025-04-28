SELECT 
  ROUND(AVG("Query_age"), 2) AS "Average_Query_Age"
FROM public."Key-insight-data-2"
WHERE "Query_age" IS NOT NULL;
