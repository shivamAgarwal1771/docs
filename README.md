SELECT 
  SUM(CASE WHEN "Resolved" = 'Yes' THEN 1 ELSE 0 END) * 100.0 / COUNT(DISTINCT "Ticket_id") * 0.01 AS c12c18436db49b40dfd77e51f1e8a51c 
FROM 
  public."Key-insight-data-2"
ORDER BY 
  c12c18436db49b40dfd77e51f1e8a51c DESC 
LIMIT 100;
