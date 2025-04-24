
SELECT 
  "Channel",
  "Intent", 
  SUM("Percentage") * 0.01 AS "Normalized_Percentage"
FROM (
  WITH top_intents AS (
    SELECT "Intent"
    FROM public."Key-insight-data-2"
    WHERE "Resolved" = 'Yes'
    GROUP BY "Intent"
    ORDER BY COUNT(*) DESC
    LIMIT 5
  ),
  labeled_data AS (
    SELECT
      "Channel",
      CASE 
        WHEN "Intent" IN (SELECT "Intent" FROM top_intents) THEN "Intent"
        ELSE 'Other'
      END AS "Intent"
    FROM public."Key-insight-data-2"
    WHERE "Resolved" = 'Yes'
  ),
  counts AS (
    SELECT 
      "Channel",
      "Intent", 
      COUNT(*) AS intent_count
    FROM labeled_data
    GROUP BY "Channel", "Intent"
  ),
  total AS (
    SELECT 
      "Channel", 
      SUM(intent_count) AS total_count 
    FROM counts
    GROUP BY "Channel"
  )
  SELECT 
    counts."Channel",
    counts."Intent",
    ROUND((counts.intent_count * 100.0) / total.total_count, 2) AS "Percentage"
  FROM counts
  JOIN total ON counts."Channel" = total."Channel"
) AS virtual_table 
GROUP BY "Channel", "Intent"
ORDER BY "Channel", "Normalized_Percentage" DESC
LIMIT 1000;
