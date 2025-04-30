SELECT 
  "Channel",
  ROUND((COUNT(*) * 100.0) / NULLIF(total_all.total_count, 0), 2) AS "Resolved_Percentage_Of_All",
  DATE_TRUNC('day', "Conversation_start_Date") AS "Conversation_start_Date",
  "Intent"
FROM 
  public."Key-insight-data-2",
  (SELECT COUNT(*) AS total_count FROM public."Key-insight-data-2") AS total_all
WHERE 
  "Resolved" = 'Yes'
GROUP BY 
  "Channel", DATE_TRUNC('day', "Conversation_start_Date"), "Intent", total_all.total_count
ORDER BY 
  "Resolved_Percentage_Of_All" DESC
LIMIT 1000;
