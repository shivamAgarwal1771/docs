SELECT
  DATE(TICKET_CREATION_TIME) AS ticket_date,
  COUNT(CASE WHEN adopted = TRUE THEN 1 END) * 100.0 / COUNT(*) AS adoption_rate
FROM your_table_name
GROUP BY DATE(TICKET_CREATION_TIME)
ORDER BY ticket_date;


SELECT
  DATE(TICKET_CREATION_TIME) AS ticket_date,
  COUNT(CASE WHEN deflected = TRUE THEN 1 END) * 100.0 / COUNT(*) AS deflection_rate
FROM your_table_name
GROUP BY DATE(TICKET_CREATION_TIME)
ORDER BY ticket_date;
