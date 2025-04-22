SELECT "Intent" AS "Intent", "Intent_useCase" AS "Intent_useCase", COUNT(*)*0.0001 AS "COUNT(*)*0.0001" 
FROM public."Key-insight-data-2" 
WHERE "Call_Type" IN ('Inbound') GROUP BY "Intent", "Intent_useCase" ORDER BY "COUNT(*)*0.0001" DESC 
 LIMIT 10000;
