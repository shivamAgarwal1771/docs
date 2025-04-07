SELECT 
  "Product", 
  "Sentiments",
  COUNT(DISTINCT "Conversation_id") * 1.0 / NULLIF(total_conversations, 0) AS "Percentage"
FROM (
  SELECT 
    "Product", 
    "Sentiments",
    "Conversation_id",
    (SELECT COUNT(DISTINCT "Conversation_id") FROM public."Sentiment_analyser_Data") AS total_conversations
  FROM public."Sentiment_analyser_Data"
) AS base
GROUP BY "Product", "Sentiments", total_conversations
ORDER BY "Percentage" DESC
LIMIT 1000
