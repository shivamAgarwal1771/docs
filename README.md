SELECT DATE_TRUNC('day', date) AS date, "Channel" AS "Channel", sum(inbound_contact_count) AS "SUM(inbound_contact_count)" 
FROM (SELECT 
    DATE("Conversation_start_time") AS date,
    "Channel",
    "Intent",
    COUNT(*) AS "inbound_contact_count"
FROM 
    public."Key-insight-data-2"
WHERE 
    "Call_Type" = 'Inbound'
GROUP BY 
    DATE("Conversation_start_time"), "Channel","Intent"
ORDER BY 
    date ASC, "Channel"
) AS virtual_table GROUP BY DATE_TRUNC('day', date), "Channel" ORDER BY "SUM(inbound_contact_count)" DESC 
 LIMIT 1000;
