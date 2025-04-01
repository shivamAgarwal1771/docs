SELECT 
    "Intent_Class", 
    (COUNT(*) * 100.0) / SUM(COUNT(*)) OVER () AS "Percentage Contacts"
FROM public."Sentiment_analyser_Data" 
GROUP BY "Intent_Class" 
ORDER BY "Percentage Contacts" DESC
LIMIT 10000;
