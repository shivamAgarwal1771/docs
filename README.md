WITH ticket_summary AS (
  SELECT
    ticket_id,
    COUNT(*) AS total_interactions,
    SUM(CASE WHEN resolved = 'yes' THEN aht ELSE 0 END) AS total_resolution_time,
    CAST(MAX(ticket_creation_time) AS DATE) AS date,  -- Extract only the date part
    MAX(channel) AS channel
  FROM your_table
  GROUP BY ticket_id
),
final_data AS (
  SELECT
    date,
    channel,
    COUNT(*) AS ticket_count,
    SUM(total_resolution_time) AS total_resolution_time
  FROM ticket_summary
  GROUP BY date, channel
)
SELECT
  date,
  channel,
  ticket_count,
  ROUND(total_resolution_time / ticket_count, 2) AS avg_resolution_time
FROM final_data
ORDER BY date;
