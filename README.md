SELECT DATE_TRUNC('day', "Conversation_start_Date") AS "Conversation_start_Date", channel AS channel, SUM("avg_resolution_time")/60 AS "SUM(""avg_resolution_time"")/60" 
FROM (WITH ticket_resolution AS (
  SELECT
    "Ticket_id",
    "Intent",
    "Conversation_start_Date",
    MAX("Channel") AS "channel",
    SUM(CASE WHEN "Resolved" = 'Yes' THEN "AHT" ELSE 0 END) AS "resolution_time"
  FROM public."Key-insight-data-2"
  GROUP BY "Ticket_id","Conversation_start_Date","Intent"
),
resolution_trend AS (
  SELECT
    "Conversation_start_Date",
    "Intent",
    "channel",
    COUNT(*) AS "unique_tickets",
    SUM("resolution_time") AS "total_resolution_time"
  FROM ticket_resolution
  GROUP BY "Conversation_start_Date", "channel","Intent"
)
SELECT
  "Conversation_start_Date",
  "channel",
  "Intent",
  "unique_tickets",
  ROUND("total_resolution_time"::NUMERIC / NULLIF("unique_tickets", 0), 2) AS "avg_resolution_time"
FROM resolution_trend
ORDER BY "Conversation_start_Date"
) AS virtual_table GROUP BY DATE_TRUNC('day', "Conversation_start_Date"), channel ORDER BY "SUM(""avg_resolution_time"")/60" DESC 
 LIMIT 1000;
