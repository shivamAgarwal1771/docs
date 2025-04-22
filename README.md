SELECT 
  "Ticket_creation_time"::date AS "Ticket_creation_date", 
  "Channel" AS "Channel", 
  COUNT(*) AS count 
FROM public."Key-insight-data-2" 
JOIN (
    SELECT 
      "Channel" AS "Channel__", 
      COUNT(*) AS mme_inner__ 
    FROM public."Key-insight-data-2" 
    WHERE "Call_Type" = 'Inbound' 
    GROUP BY "Channel" 
    ORDER BY COUNT("Channel") DESC 
    LIMIT 5
) AS series_limit 
ON "Channel" = "Channel__" 
WHERE "Call_Type" = 'Inbound' 
GROUP BY "Ticket_creation_date", "Channel" 
ORDER BY count DESC 
LIMIT 10000;
