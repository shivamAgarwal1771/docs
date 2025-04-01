SELECT "Product" AS "Product", "Sentiments" AS "Sentiments", COUNT(*)*0.01 AS "COUNT(*)*0.01" 
FROM public."Sentiment_analyser_Data" GROUP BY "Product", "Sentiments" ORDER BY "COUNT(*)*0.01" DESC 
 LIMIT 10000;
