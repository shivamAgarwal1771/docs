SELECT 
  "Channel",
  SUM("Agent Utilization") AS "Agent Utilization"
FROM (
  SELECT 
    "Channel",
    DATE("Ticket_creation_time") AS "Date",
    SUM("AHT") + SUM("Agent_ACW") AS "Total_Handle_and_Work_Time",
    SUM("Agent_Shift_Time") AS "Total_Shift_Hours",
    (SUM("AHT") + SUM("Agent_ACW")) / NULLIF(SUM("Agent_Shift_Time"), 0) AS "Agent Utilization"
  FROM 
    public."Key-insight-data-2"
  GROUP BY 
    "Channel", DATE("Ticket_creation_time")
  ORDER BY 
    "Date" ASC
  LIMIT 10000
) AS virtual_table
GROUP BY "Channel"
ORDER BY "Agent Utilization" DESC 
LIMIT 100;
