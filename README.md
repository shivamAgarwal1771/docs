SELECT 
  "Intent",
  "Channel",
  DATE_TRUNC('day', "Conversation_start_Date") AS "Conversation_start_Date",
  ROUND((COUNT(DISTINCT "Ticket_id") * 100.0) / NULLIF(total_inbound.total_count, 0), 2) AS "Resolved_Intent_Percentage"
FROM 
  public."Key-insight-data-2",
  (SELECT COUNT(DISTINCT "Ticket_id") AS total_count 
   FROM public."Key-insight-data-2" 
   WHERE "Call_Type" = 'Inbound' AND "Resolved" = 'Yes') AS total_inbound
WHERE 
  "Call_Type" = 'Inbound' AND "Resolved" = 'Yes'
GROUP BY 
  "Intent", "Channel", DATE_TRUNC('day', "Conversation_start_Date"), total_inbound.total_count
ORDER BY 
  "Resolved_Intent_Percentage" DESC
LIMIT 1000;
