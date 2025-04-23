SELECT 
    DATE(Ticket_creation_time) AS date,
    COUNT(*) AS inbound_contact_count
FROM 
    your_table_name
WHERE 
    Call_Type = 'Inbound'
GROUP BY 
    DATE(Ticket_creation_time)
ORDER BY 
    date ASC;
