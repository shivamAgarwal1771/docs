SELECT "Ticket_creation_time" AS "Ticket_creation_time", "Channel" AS "Channel", AVG("AHT")/60 AS "AVG(""AHT"")/60" 
FROM public."Key-insight-data-2" JOIN (SELECT "Channel" AS "Channel__", AVG("AHT")/60 AS mme_inner__ 
FROM public."Key-insight-data-2" 
WHERE "Resolved" IN ('Yes') GROUP BY "Channel" ORDER BY mme_inner__ DESC 
 LIMIT 10) AS series_limit ON "Channel" = "Channel__" 
WHERE "Resolved" IN ('Yes') GROUP BY "Ticket_creation_time", "Channel" ORDER BY "AVG(""AHT"")/60" DESC 
 LIMIT 10000;
