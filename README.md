SELECT 
  "Intent",
  "Channel",
  DATE_TRUNC('day', "Conversation_start_Date") AS "Conversation_start_Date",
  ROUND(AVG("Query_age") / 60.0, 2) AS "Average_Query_Age_in_Minutes"
FROM 
  public."Key-insight-data-2"
GROUP BY 
  "Intent",
  "Channel",
  DATE_TRUNC('day', "Conversation_start_Date")
ORDER BY 
  "Average_Query_Age_in_Minutes" DESC
LIMIT 50000;
