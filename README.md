WITH intent_counts AS (
    SELECT
        intent,
        COUNT(*) AS intent_count,
        COUNT(*) * 100.0 / (SELECT COUNT(*) FROM inbound_contacts) AS intent_percentage
    FROM inbound_contacts
    GROUP BY intent
)

SELECT
    intent,
    intent_count,
    intent_percentage,
    channel,
    DATE(created_at) AS date
FROM intent_counts
JOIN inbound_contacts ON inbound_contacts.intent = intent_counts.intent
GROUP BY intent, channel, date
ORDER BY date, intent;
