SELECT 
    "Reason", 
    COUNT(*) AS "inbound_contact_count"
FROM 
    public."Key-insight-data-2"
WHERE 
    "Call_Type" = 'Inbound'
GROUP BY 
    "Reason"
ORDER BY 
    "inbound_contact_count" DESC
LIMIT 1000;
