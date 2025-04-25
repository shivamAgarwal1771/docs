WITH ticket_aht AS (
  SELECT 
    "Ticket_id",
    SUM("AHT") AS total_aht  -- sum AHTs for repeated tickets
  FROM public."Key-insight-data-2"
  WHERE "Resolved" = 'Yes'
  GROUP BY "Ticket_id"
),
final_summary AS (
  SELECT 
    COUNT(*) AS unique_ticket_count,
    SUM(total_aht) AS total_resolution_time_in_sec
  FROM ticket_aht
)
SELECT 
  ROUND(total_resolution_time_in_sec / 60.0, 2) AS total_resolution_time_in_minutes,
  unique_ticket_count,
  ROUND((total_resolution_time_in_sec / unique_ticket_count) / 60.0, 2) AS avg_aht_per_ticket_in_minutes
FROM final_summary;
