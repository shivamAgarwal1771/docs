SELECT 
    A."Conversation_start_date" AS date,
    A."Channel",
    A."Intent",
    COUNT(*) AS intent_count,
    (COUNT(*) * 100.0 / total.total_count) AS intent_percentage
FROM 
    public."Key-insight-data-2" A
JOIN (
    SELECT 
        "Conversation_start_date",
        "Channel",
        COUNT(*) AS total_count
    FROM 
        public."Key-insight-data-2"
    WHERE 
        "Call_Type" = 'Inbound'
    GROUP BY 
        "Conversation_start_date", "Channel"
) total
ON 
    A."Conversation_start_date" = total."Conversation_start_date"
    AND A."Channel" = total."Channel"
WHERE 
    A."Call_Type" = 'Inbound'
GROUP BY 
    A."Conversation_start_date", A."Channel", A."Intent", total.total_count
ORDER BY 
    date ASC, A."Channel", intent_percentage DESC;
