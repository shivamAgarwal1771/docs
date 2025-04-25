SELECT "Intent" AS "Intent", SUM("intent_percentage")*0.001 AS "Percentage" 
FROM (SELECT 
    A."Conversation_start_Date" AS date,
    A."Channel",
    A."Intent",
    COUNT(*) AS intent_count,
    (COUNT(*) * 100.0 / total.total_count) AS intent_percentage
FROM 
    public."Key-insight-data-2" A
JOIN (
    SELECT 
        "Conversation_start_Date",
        "Channel",
        COUNT(*) AS total_count
    FROM 
        public."Key-insight-data-2"
    WHERE 
        "Call_Type" = 'Inbound'
    GROUP BY 
        "Conversation_start_Date", "Channel"
) total
ON 
    A."Conversation_start_Date" = total."Conversation_start_Date"
    AND A."Channel" = total."Channel"
WHERE 
    A."Call_Type" = 'Inbound'
GROUP BY 
    A."Conversation_start_Date", A."Channel", A."Intent", total.total_count
ORDER BY 
    date ASC, A."Channel", intent_percentage DESC
) AS virtual_table GROUP BY "Intent" ORDER BY "Percentage" DESC 
 LIMIT 1000;
