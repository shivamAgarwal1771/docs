SELECT 
    conversation_start_date AS date, 
    "Channel", 
    SUM(inbound_contact_count) AS "SUM(inbound_contact_count)"
FROM (
    SELECT 
        conversation_start_date,
        "Channel",
        COUNT(*) AS inbound_contact_count
    FROM 
        public."Key-insight-data-2"
    WHERE 
        "Call_Type" = 'Inbound'
    GROUP BY 
        conversation_start_date, "Channel"
) AS virtual_table 
GROUP BY 
    conversation_start_date, "Channel"
ORDER BY 
    "SUM(inbound_contact_count)" DESC 
LIMIT 1000;
