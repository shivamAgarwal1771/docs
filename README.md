SELECT 
  DATE("Ticket_creation_time") AS "Date", 
  "Channel", 
  "Intent", 
  ROUND((COUNT(*) * 100.0) / NULLIF(total_inbound.total_count, 0), 2) AS "Percentage"
FROM 
  public."Key-insight-data-2",
  (SELECT COUNT(*) AS total_count FROM public."Key-insight-data-2" WHERE "Call_Type" = 'Inbound') AS total_inbound
WHERE 
  "Call_Type" = 'Inbound'
GROUP BY 
  DATE("Ticket_creation_time"), "Channel", "Intent", total_inbound.total_count
ORDER BY 
  "Percentage" DESC
LIMIT 1000;
