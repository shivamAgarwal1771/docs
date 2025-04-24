SELECT 
  DATE("Ticket_creation_time") AS "Date",
  SUM("AHT") + SUM("ACW") AS "Total_Handle_and_Work_Time",
  SUM("SHIFT_HOURS") AS "Total_Shift_Hours",
  (SUM("AHT") + SUM("ACW")) / NULLIF(SUM("SHIFT_HOURS"), 0) AS "Agent Utilization"
FROM 
  public."Key-insight-data-2"
GROUP BY 
  DATE("Ticket_creation_time")
ORDER BY 
  "Date" ASC
LIMIT 10000;
