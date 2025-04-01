SELECT 
    "Product", 
    "Sentiments", 
    COUNT(*) * 1.0 / SUM(COUNT(*)) OVER (PARTITION BY "Product") AS "Percentage"
FROM public."Sentiment_analyser_Data" 
GROUP BY "Product", "Sentiments"
ORDER BY "Percentage" DESC 
LIMIT 10000;
