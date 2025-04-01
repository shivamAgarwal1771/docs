SELECT "Intent_Class" AS "Intent_Class", COUNT(*)*0.001 AS "Percentage Contacts" 
FROM public."Sentiment_analyser_Data" GROUP BY "Intent_Class" 
 LIMIT 10000;
