SELECT 
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
LIMIT 1000;
