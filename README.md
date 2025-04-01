SELECT 
    "Product", 
    "CSAT_Bucket", 
    COUNT("CSAT_Bucket") * 1.0 / SUM(COUNT("CSAT_Bucket")) OVER (PARTITION BY "Product") AS "Percentage"
FROM public."Sentiment_analyser_Data" 
GROUP BY "Product", "CSAT_Bucket"
ORDER BY "Percentage" DESC 
LIMIT 10000;
