WITH resolved_tickets AS (
  SELECT
    "Ticket_id",
    CAST(MAX("Ticket_creation_time") AS DATE) AS "date",
    MAX("Channel") AS "channel",
    SUM(CASE WHEN "Resolved" = 'yes' THEN "AHT" ELSE 0 END) AS "resolution_time",
    MAX(CASE WHEN "Resolved" = 'yes' THEN 1 ELSE 0 END) AS is_resolved
  FROM public."Key-insight-data-2"
  GROUP BY "Ticket_id"
),
filtered_resolved AS (
  SELECT
    *
  FROM resolved_tickets
  WHERE is_resolved = 1 -- keep only resolved tickets
),
resolution_trend AS (
  SELECT
    "date",
    "channel",
    COUNT(*) AS "resolved_tickets",
    SUM("resolution_time") AS "total_resolution_time"
  FROM filtered_resolved
  GROUP BY "date", "channel"
)
SELECT
  "date",
  "channel",
  ROUND("total_resolution_time"::NUMERIC / NULLIF("resolved_tickets", 0), 2) AS "avg_resolution_time"
FROM resolution_trend
ORDER BY "date";
