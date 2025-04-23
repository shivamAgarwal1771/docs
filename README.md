SELECT "Intent" AS "Intent", COUNT(*)*3*0.001 AS "COUNT(*)*3*0.001" 
FROM public."Key-insight-data-2" 
WHERE "Resolved" IN ('Yes') GROUP BY "Intent" ORDER BY "COUNT(*)*3*0.001" DESC 
 LIMIT 10000;
