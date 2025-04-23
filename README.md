WITH top_intents AS (
  SELECT "Intent"
  FROM public."Key-insight-data-2"
  WHERE "Call_Type" = 'Inbound'
  GROUP BY "Intent"
  ORDER BY COUNT(*) DESC
  LIMIT 5
)
SELECT
  CASE 
    WHEN "Intent" IN (SELECT "Intent" FROM top_intents) THEN "Intent"
    ELSE 'Other'
  END AS "Intent",
  COUNT(*) * 0.0001 AS "COUNT(*)*0.0001"
FROM public."Key-insight-data-2"
WHERE "Call_Type" = 'Inbound'
GROUP BY 
  CASE 
    WHEN "Intent" IN (SELECT "Intent" FROM top_intents) THEN "Intent"
    ELSE 'Other'
  END
ORDER BY "COUNT(*)*0.0001" DESC;
