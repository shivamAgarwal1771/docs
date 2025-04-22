SELECT "Ticket_creation_time" AS "Ticket_creation_time", "Channel" AS "Channel", COUNT(*) AS count 
FROM public."Key-insight-data-2" JOIN (SELECT "Channel" AS "Channel__", COUNT(*) AS mme_inner__ 
FROM public."Key-insight-data-2" 
WHERE "Call_Type" = 'Inbound' GROUP BY "Channel" ORDER BY count("Channel") DESC 
 LIMIT 5) AS series_limit ON "Channel" = "Channel__" 
WHERE "Call_Type" = 'Inbound' GROUP BY "Ticket_creation_time", "Channel" ORDER BY count("Channel") DESC 
 LIMIT 10000;
