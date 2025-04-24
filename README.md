WITH ticket_resolution AS (
  SELECT
    "Ticket_id",
    CAST(MAX("Ticket_creation_time") AS DATE) AS "date",
    MAX("Channel") AS "channel",
    SUM(CASE WHEN "Resolved" = 'yes' THEN "AHT" ELSE 0 END) AS "resolution_time"
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
ORDER BY "date";
