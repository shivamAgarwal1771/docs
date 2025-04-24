WITH "ticket_summary" AS (
  SELECT
    "Ticket_id",
    COUNT(*) AS "total_interactions",
    SUM(CASE 
          WHEN LOWER("Resolved") = 'yes' THEN COALESCE("AHT", 0) 
          ELSE 0 
        END) AS "total_resolution_time",
    CAST(MAX("Ticket_creation_time") AS DATE) AS "date",
    MAX("Channel") AS "channel"
  FROM public."Key-insight-data-2"
  GROUP BY "Ticket_id"
),
"final_data" AS (
  SELECT
    "date",
    "channel",
    COUNT(*) AS "ticket_count",
    SUM("total_resolution_time") AS "total_resolution_time"
  FROM "ticket_summary"
  GROUP BY "date", "channel"
)
SELECT
  "date",
  "channel",
  "ticket_count",
  ROUND("total_resolution_time" * 1.0 / NULLIF("ticket_count", 0), 2) AS "avg_resolution_time"
FROM "final_data"
ORDER BY "date";
