SELECT "Product" AS "Product", "CSAT_Bucket" AS "CSAT_Bucket", COUNT("CSAT_Bucket")*0.001 AS "COUNT(""CSAT_Bucket"")*0.001" 
FROM public."Sentiment_analyser_Data" GROUP BY "Product", "CSAT_Bucket" ORDER BY "COUNT(""CSAT_Bucket"")*0.001" DESC 
 LIMIT 10000;
