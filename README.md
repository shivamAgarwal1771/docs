SELECT 
    "conversation_start_date" AS date,
    "Channel",
    "Reason", 
    COUNT(*) AS inbound_contact_count
FROM 
    public."Key-insight-data-2"
WHERE 
    "Call_Type" = 'Inbound'
GROUP BY 
    "conversation_start_date", "Channel", "Reason"
ORDER BY 
    date ASC, inbound_contact_count DESC
LIMIT 1000;
