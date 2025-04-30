SELECT "Intent" AS "Intent", SUM("Percentage")*0.01 AS "Percentage" 
FROM (SELECT 
  "Conversation_start_Date", 
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
 LIMIT 5;
