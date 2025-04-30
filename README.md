SELECT 
  "Channel",
  DATE_TRUNC('day', "Conversation_start_Date") AS "Conversation_start_Date",
  "Intent",
  ROUND((COUNT(*) * 100.0) / NULLIF(total_resolved.total_count, 0), 2) AS "Resolved_Percentage"
FROM 
  public."Key-insight-data-2",
  (SELECT COUNT(*) AS total_count 
   FROM public."Key-insight-data-2" 
   WHERE "Resolved" = 'Yes') AS total_resolved
WHERE 
  "Resolved" = 'Yes'
GROUP BY 
  "Channel", 
  DATE_TRUNC('day', "Conversation_start_Date"), 
  "Intent", 
  total_resolved.total_count
ORDER BY 
  "Resolved_Percentage" DESC
LIMIT 1000;
