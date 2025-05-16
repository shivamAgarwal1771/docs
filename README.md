SELECT "Intent" AS "Intent", sum("SUM(Per)") AS "SUM(SUM(Per))" 
FROM (SELECT "Intent" AS "Intent","Product" As "Product", sum("Per") AS "SUM(Per)" , "Date", "Agent_id", "Intent_Class"
FROM (SELECT
"A"."Intent_Class", "A"."Intent", "A"."Intent" as "Intent2", "Agent_id","Product","Date",
"A"."Conversation_I",
("A"."Conversation_I"::FLOAT/NULLIF((SELECT 
COUNT(DISTINCT "Conversation_id")
FROM public."Sentiment_analyser_Data"),0)) AS "Per"
FROM 
--("Total"::FLOAT/NULLIF("Ticket_Ids",0))*100
(
SELECT 
"Intent_Class","Intent", "Agent_id","Product", "Date",
COUNT(DISTINCT "Conversation_id") AS "Conversation_I"
FROM public."Sentiment_analyser_Data"
GROUP BY 
"Intent_Class","Intent", "Agent_id","Product", "Date") AS "A"
) AS virtual_table GROUP BY "Intent","Intent_Class","Product" , "Date", "Agent_id" ORDER BY "SUM(Per)" DESC 
 LIMIT 1000
) AS virtual_table GROUP BY "Intent" ORDER BY "SUM(SUM(Per))" DESC 
 LIMIT 1000;



SELECT "Intent" AS "Intent", "Conversation_Sentiment" AS "Conversation_Sentiment", sum("Per") AS "SUM(Per)" 
FROM (SELECT  A."Intent", A."Intent_Class",A."Agent_id", A."Product", A."Date", A."Conversation_Sentiment", 
(A."Conversations"::FLOAT/NULLIF((B."Conversation_I"),0)) AS "Per"

FROM 
(SELECT  "Intent", "Intent_Class","Agent_id","Product", "Date", "Conversation_Sentiment", COUNT(DISTINCT "Conversation_id") AS "Conversations"
FROM public."Sentiment_analyser_Data"
GROUP BY "Intent", "Intent_Class","Agent_id","Product", "Date", "Conversation_Sentiment") as A
left join 
(SELECT  "Intent", COUNT(DISTINCT "Conversation_id") AS "Conversation_I"
FROM public."Sentiment_analyser_Data"
GROUP BY "Intent") as B
on A."Intent" = B."Intent"
) AS virtual_table GROUP BY "Intent", "Conversation_Sentiment" ORDER BY "SUM(Per)" DESC 
 LIMIT 1000;



SELECT "Intent" AS "Intent", "CSAT_Bucket" AS "CSAT_Bucket", sum("Per") AS "SUM(Per)" 
FROM (SELECT  A."Intent",A."Intent_Class", A."Agent_id", A."Product", A."Date", A."CSAT_Bucket", 
(A."Conversations"::FLOAT/NULLIF((B."Conversation_I"),0)) AS "Per"

FROM 
(SELECT  "Intent", "Intent_Class","Agent_id","Product", "Date", "CSAT_Bucket", COUNT(DISTINCT "Conversation_id") AS "Conversations"
FROM public."Sentiment_analyser_Data"
GROUP BY "Intent","Intent_Class","Agent_id","Product", "Date", "CSAT_Bucket") as A
left join 
(SELECT  "Intent", COUNT(DISTINCT "Conversation_id") AS "Conversation_I"
FROM public."Sentiment_analyser_Data"
GROUP BY "Intent") as B
on A."Intent" = B."Intent"
) AS virtual_table GROUP BY "Intent", "CSAT_Bucket" ORDER BY "SUM(Per)" DESC 
 LIMIT 1000;
