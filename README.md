SELECT DATE_TRUNC('day', date) AS date, "Channel" AS "Channel", sum(inbound_contact_count) AS "SUM(inbound_contact_count)" 
FROM (SELECT 
    DATE("Ticket_creation_time") AS date,
    "Channel",
    COUNT(*) AS "inbound_contact_count"
FROM 
    public."Key-insight-data-2"
WHERE 
    "Call_Type" = 'Inbound'
GROUP BY 
    DATE("Ticket_creation_time"), "Channel"
ORDER BY 
    date ASC, "Channel"
) AS virtual_table GROUP BY DATE_TRUNC('day', date), "Channel" ORDER BY "SUM(inbound_contact_count)" DESC 
 LIMIT 1000;





SELECT "Intent" AS "Intent", SUM("Percentage")*0.01 AS "Percentage" 
FROM (WITH top_intents AS (
  SELECT "Intent"
  FROM public."Key-insight-data-2"
  WHERE "Call_Type" = 'Inbound'
  GROUP BY "Intent"
  ORDER BY COUNT(*) DESC
  LIMIT 5
),
labeled_data AS (
  SELECT
    CASE 
      WHEN "Intent" IN (SELECT "Intent" FROM top_intents) THEN "Intent"
      ELSE 'Other'
    END AS "Intent"
  FROM public."Key-insight-data-2"
  WHERE "Call_Type" = 'Inbound'
),
counts AS (
  SELECT 
    "Intent", 
    COUNT(*) AS intent_count
  FROM labeled_data
  GROUP BY "Intent"
),
total AS (
  SELECT SUM(intent_count) AS total_count FROM counts
)
SELECT 
  counts."Intent",
  ROUND((counts.intent_count * 100.0) / total.total_count, 2) AS "Percentage"
FROM counts, total
ORDER BY "Percentage" DESC
) AS virtual_table GROUP BY "Intent" ORDER BY "Percentage" DESC 
 LIMIT 1000;




SELECT sum("Agent Utilization") AS "Agent Utilization" 
FROM (SELECT 
  DATE("Ticket_creation_time") AS "Date",
  SUM("AHT") + SUM("Agent_ACW") AS "Total_Handle_and_Work_Time",
  SUM("Agent_Shift_Time") AS "Total_Shift_Hours",
  (SUM("AHT") + SUM("Agent_ACW")) / NULLIF(SUM("Agent_Shift_Time"), 0) AS "Agent Utilization"
FROM 
  public."Key-insight-data-2"
GROUP BY 
  DATE("Ticket_creation_time")
ORDER BY 
  "Date" ASC
LIMIT 10000
) AS virtual_table ORDER BY "Agent Utilization" DESC 
 LIMIT 100;



SELECT DATE_TRUNC('day', date) AS date, channel AS channel, sum(avg_resolution_time) AS "SUM(avg_resolution_time)" 
FROM (WITH ticket_resolution AS (
  SELECT
    "Ticket_id",
    CAST(MAX("Ticket_creation_time") AS DATE) AS "date",
    MAX("Channel") AS "channel",
    SUM(CASE WHEN "Resolved" = 'Yes' THEN "AHT" ELSE 0 END) AS "resolution_time"
  FROM public."Key-insight-data-2"
  GROUP BY "Ticket_id"
),
resolution_trend AS (
  SELECT
    "date",
    "channel",
    COUNT(*) AS "unique_tickets",
    SUM("resolution_time") AS "total_resolution_time"
  FROM ticket_resolution
  GROUP BY "date", "channel"
)
SELECT
  "date",
  "channel",
  "unique_tickets",
  ROUND("total_resolution_time"::NUMERIC / NULLIF("unique_tickets", 0), 2) AS "avg_resolution_time"
FROM resolution_trend
ORDER BY "date"
) AS virtual_table GROUP BY DATE_TRUNC('day', date), channel ORDER BY "SUM(avg_resolution_time)" DESC 
 LIMIT 1000;


SELECT DATE("Ticket_creation_time") AS "My column", SUM(CASE WHEN "Conv_AI_Adoption" = 'Yes' THEN 1 ELSE 0 END)*100/ COUNT(*)*0.01 AS "Conv AI", SUM(CASE WHEN "IVR_Adoption" = 'Yes' THEN 1 ELSE 0 END)*100/ COUNT(*)*0.01 AS "IVR" 
FROM public."Key-insight-data-2" GROUP BY DATE("Ticket_creation_time") ORDER BY "Conv AI" DESC 
 LIMIT 10000;
