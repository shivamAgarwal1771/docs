WITH ticket_aht AS (
  SELECT 
    "Ticket_id",
    DATE_TRUNC('day', "Conversation_start_Date") AS "Conversation_start_Date",  -- Group by day
    SUM("AHT") AS total_aht  -- Sum AHTs for repeated tickets
  FROM public."Key-insight-data-2"
  WHERE "Resolved" = 'Yes'
  GROUP BY "Ticket_id", DATE_TRUNC('day', "Conversation_start_Date")
),
final_summary AS (
  SELECT 
    "Conversation_start_Date",
    COUNT(*) AS unique_ticket_count,
    SUM(total_aht) AS total_resolution_time_in_sec
  FROM ticket_aht
  GROUP BY "Conversation_start_Date"
)
SELECT 
  "Conversation_start_Date",
  ROUND(total_resolution_time_in_sec / 60.0, 2) AS total_resolution_time_in_minutes,
  unique_ticket_count,
  ROUND((total_resolution_time_in_sec / unique_ticket_count) / 60.0, 2) AS avg_aht_per_ticket_in_minutes
FROM final_summary
ORDER BY "Conversation_start_Date" DESC
LIMIT 1000;
