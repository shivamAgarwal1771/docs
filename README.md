SELECT "Product" AS "Product", sum("Per") AS "SUM(Per)" 
FROM (SELECT
"A"."Product", 
"A"."Conversation_I",
("A"."Conversation_I"::FLOAT/NULLIF((SELECT 
COUNT(DISTINCT "Conversation_id")
FROM public."Sentiment_analyser_Data"),0)) AS "Per"
FROM 
--("Total"::FLOAT/NULLIF("Ticket_Ids",0))*100
(
SELECT 
"Product",
COUNT(DISTINCT "Conversation_id") AS "Conversation_I"
FROM public."Sentiment_analyser_Data"
GROUP BY 
"Product") AS "A"
) AS virtual_table GROUP BY "Product" ORDER BY "SUM(Per)" DESC 
 LIMIT 1000;



SELECT "Product" AS "Product", "Sentiments" AS "Sentiments", sum("Percentage") AS "SUM(Percentage)" 
FROM (SELECT 
    "Product", 
    "Sentiments", 
    COUNT(*) * 1.0 / SUM(COUNT(*)) OVER (PARTITION BY "Product") AS "Percentage"
FROM public."Sentiment_analyser_Data" 
GROUP BY "Product", "Sentiments"
ORDER BY "Percentage" DESC 
LIMIT 10000
) AS virtual_table GROUP BY "Product", "Sentiments" ORDER BY "SUM(Percentage)" DESC 
 LIMIT 1000;
