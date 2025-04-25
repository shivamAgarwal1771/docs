SELECT DATE_TRUNC('day', date) AS date, "Channel" AS "Channel", sum("SUM(inbound_contact_count)") AS "SUM(SUM(inbound_contact_count))" 
FROM (SELECT 
    "Conversation_start_Date" AS date, 
    "Channel", 
    "Intent",
    SUM("inbound_contact_count") AS "SUM(inbound_contact_count)"
FROM (
    SELECT 
        "Conversation_start_Date",
        "Channel",
        "Intent",
        COUNT(*) AS "inbound_contact_count"
    FROM 
        public."Key-insight-data-2"
    WHERE 
        "Call_Type" = 'Inbound'
    GROUP BY 
        "Conversation_start_Date", "Channel", "Intent"
) AS virtual_table 
GROUP BY 
    "Conversation_start_Date", "Channel", "Intent"
ORDER BY 
    "SUM(inbound_contact_count)" DESC
) AS virtual_table GROUP BY DATE_TRUNC('day', date), "Channel" ORDER BY "SUM(SUM(inbound_contact_count))" DESC 
 LIMIT 1000;








SELECT "Intent" AS "Intent", SUM("Percentage")*0.01 AS "Percentage" 
FROM (SELECT 
  "Conversation_start_Date" AS "Date", 
  "Channel", 
  "Intent", 
  ROUND((COUNT(*) * 100.0) / NULLIF(total_inbound.total_count, 0), 2) AS "Percentage"
FROM 
  public."Key-insight-data-2",
  (SELECT COUNT(*) AS total_count FROM public."Key-insight-data-2" WHERE "Call_Type" = 'Inbound') AS total_inbound
WHERE 
  "Call_Type" = 'Inbound'
GROUP BY 
  "Conversation_start_Date", "Channel", "Intent", total_inbound.total_count
ORDER BY 
  "Percentage" DESC
LIMIT 1000
) AS virtual_table GROUP BY "Intent" ORDER BY "Percentage" DESC 
 LIMIT 1000;







SELECT SUM("Agent Utilization")*0.01 AS "Agent Utilization" 
FROM (SELECT 
  "Conversation_start_Date" AS "Date", 
  "Channel", 
  "Intent", 
  agent_utilization."Agent Utilization"
FROM (
  SELECT 
    SUM("AHT") + SUM("Agent_ACW") AS "Total_Handle_and_Work_Time",
    SUM("Agent_Shift_Time") AS "Total_Shift_Hours",
    (SUM("AHT") + SUM("Agent_ACW")) / NULLIF(SUM("Agent_Shift_Time"), 0) AS "Agent Utilization"
  FROM 
    public."Key-insight-data-2"
) AS agent_utilization,
public."Key-insight-data-2" AS data
WHERE 
  data."Call_Type" = 'Inbound'
GROUP BY 
  "Conversation_start_Date", "Channel", "Intent", agent_utilization."Agent Utilization"
ORDER BY 
  "Date" ASC
LIMIT 1000
) AS virtual_table ORDER BY "Agent Utilization" DESC 
 LIMIT 100;






SELECT DATE_TRUNC('day', "Conversation_start_Date") AS "Conversation_start_Date", "Channel" AS "Channel", AVG("avg_aht_per_ticket_in_minutes") AS "AVG(""avg_aht_per_ticket_in_minutes"")" 
FROM (WITH ticket_aht AS (
  SELECT 
    "Ticket_id",
    "Conversation_start_Date",  
    "Channel",
    "Intent",
    SUM("AHT") AS total_aht  
  FROM public."Key-insight-data-2"
  WHERE "Resolved" = 'Yes'
  GROUP BY "Ticket_id", "Conversation_start_Date","Channel","Intent"
),
final_summary AS (
  SELECT 
    "Conversation_start_Date",
    "Channel",
    "Intent",
    COUNT(*) AS unique_ticket_count,
    SUM(total_aht) AS total_resolution_time_in_sec
  FROM ticket_aht
  GROUP BY "Conversation_start_Date","Channel","Intent"
)
SELECT 
  "Conversation_start_Date",
  "Channel",
  "Intent",
  ROUND(total_resolution_time_in_sec / 60.0, 2) AS total_resolution_time_in_minutes,
  unique_ticket_count,
  ROUND((total_resolution_time_in_sec / unique_ticket_count) / 60.0, 2) AS avg_aht_per_ticket_in_minutes
FROM final_summary
ORDER BY "Conversation_start_Date" DESC
LIMIT 1000
) AS virtual_table GROUP BY DATE_TRUNC('day', "Conversation_start_Date"), "Channel" ORDER BY "AVG(""avg_aht_per_ticket_in_minutes"")" DESC 
 LIMIT 1000;
