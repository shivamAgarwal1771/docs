SELECT 
  "Conversation_start_Date" AS "Date", 
  "Channel", 
  "Intent", 
  SUM("Agent Utilization") AS "Agent Utilization" 
FROM (
  SELECT 
    "Conversation_start_Date" AS "Date",
    "Channel",
    "Intent",
    SUM("AHT") + SUM("Agent_ACW") AS "Total_Handle_and_Work_Time",
    SUM("Agent_Shift_Time") AS "Total_Shift_Hours",
    (SUM("AHT") + SUM("Agent_ACW")) / NULLIF(SUM("Agent_Shift_Time"), 0) AS "Agent Utilization"
  FROM 
    public."Key-insight-data-2"
  GROUP BY 
    "Conversation_start_Date", "Channel", "Intent"
  ORDER BY 
    "Conversation_start_Date" ASC
  LIMIT 10000
) AS virtual_table 
GROUP BY 
  "Conversation_start_Date", "Channel", "Intent" 
ORDER BY 
  "Agent Utilization" DESC 
LIMIT 100;
