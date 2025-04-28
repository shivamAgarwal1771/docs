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




SELECT "Intent" AS "Intent", SUM("Percentage")*0.01 AS "Percentage" 
FROM (SELECT 
  "Conversation_start_Date" AS "Date", 
  "Channel", 
  "Intent", 
  ROUND((COUNT(*) * 100.0) / NULLIF(total_inbound.total_count, 0), 2) AS "Percentage"
FROM 
  public."Key-insight-data-2",
  (SELECT COUNT(*) AS total_count FROM public."Key-insight-data-2" WHERE "Call_Type" = 'Inbound') AS total_inbound
WHERE 
  "Call_Type" = 'Inbound'
GROUP BY 
  "Conversation_start_Date", "Channel", "Intent", total_inbound.total_count
ORDER BY 
  "Percentage" DESC
LIMIT 1000
) AS virtual_table GROUP BY "Intent" ORDER BY "Percentage" DESC 
 LIMIT 1000;

