SELECT 
  DATE_TRUNC('day', "Conversation_start_Date") AS "Conversation_start_Date", 
  "Channel",
  "Intent",
  AVG("avg_resolution_time") / 60 AS "AVG(avg_resolution_time)/60"
FROM (
  WITH ticket_aht AS (
    SELECT 
      "Ticket_id",
      "Conversation_start_Date",  
      "Channel",
      "Intent",
      SUM("AHT") AS total_aht  
    FROM public."Key-insight-data-2"
    WHERE "Resolved" = 'Yes'
    GROUP BY "Ticket_id", "Conversation_start_Date", "Channel", "Intent"
  ),
  final_summary AS (
    SELECT 
      "Conversation_start_Date",
      "Channel",
      "Intent",
      COUNT(*) AS unique_ticket_count,
      SUM(total_aht) AS total_resolution_time_in_sec
    FROM ticket_aht
    GROUP BY "Conversation_start_Date", "Channel", "Intent"
  )
  SELECT 
    "Conversation_start_Date",
    "Channel",
    "Intent",
    ROUND(total_resolution_time_in_sec / 60.0, 2) AS total_resolution_time_in_minutes,
    unique_ticket_count,
    ROUND((total_resolution_time_in_sec / unique_ticket_count) / 60.0, 2) AS avg_aht_per_ticket_in_minutes
  FROM final_summary
  ORDER BY "Conversation_start_Date" DESC
  LIMIT 1000
) AS virtual_table
GROUP BY DATE_TRUNC('day', "Conversation_start_Date"), "Channel", "Intent"
ORDER BY "AVG(avg_resolution_time)/60" DESC 
LIMIT 1000;
