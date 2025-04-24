SELECT 
  SUM("Agent Utilization") AS "Agent Utilization"
FROM (
  SELECT 
    DATE("Ticket_creation_time") AS "Date",
    "Channel",  -- Just fetching it, not grouping or aggregating by it
    SUM("AHT") + SUM("Agent_ACW") AS "Total_Handle_and_Work_Time",
    SUM("Agent_Shift_Time") AS "Total_Shift_Hours",
    (SUM("AHT") + SUM("Agent_ACW")) / NULLIF(SUM("Agent_Shift_Time"), 0) AS "Agent Utilization"
  FROM 
    public."Key-insight-data-2"
  GROUP BY 
    DATE("Ticket_creation_time"), "Channel"
  ORDER BY 
    "Date" ASC
  LIMIT 10000
) AS virtual_table 
ORDER BY "Agent Utilization" DESC 
LIMIT 100;
