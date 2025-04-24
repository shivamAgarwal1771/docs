SELECT "Ticket_creation_time" AS "Ticket_creation_time", "Channel" AS "Channel", AVG("Cost_per_Contact") AS "AVG(Cost_per_Contact)" 
FROM public."Key-insight-data-2" GROUP BY "Ticket_creation_time", "Channel" ORDER BY "AVG(Cost_per_Contact)" DESC 
 LIMIT 10000;
