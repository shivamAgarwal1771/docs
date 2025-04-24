SELECT DATE("Ticket_creation_time") AS "Ticket_creation_time",   COUNT(CASE WHEN "Conv_AI_Adoption" = 'Yes' THEN 1 END) * 100.0 / COUNT(*)  AS "Conv AI ",   COUNT(CASE WHEN "IVR_Adoption" = 'Yes' THEN 1 END) * 100.0 / COUNT(*)  AS "IVR" 
FROM public."Key-insight-data-2" GROUP BY DATE("Ticket_creation_time") ORDER BY "Conv AI " DESC 
 LIMIT 10000;
