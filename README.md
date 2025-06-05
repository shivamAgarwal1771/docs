SELECT
    "A"."Emp_ID",
    "A"."Conversation_start_Date",
    "A"."Channel",
    "A"."BU_Name",
    "A"."Intent", "Total_Handle_and_Work_Time", "Total_Shift_Hours", "Conversations",
    COUNT DISTINCT("B"."Emp_ID") AS "Agents",
    COUNT DISTINCT("B"."Conversation_start_Date") as "Days"
FROM
(
    public."Key-insight-data-2" AS "B"
LEFT JOIN
(
SELECT 
    "Emp_ID",
    "Conversation_start_Date",
    "Channel",
    "BU_Name",
    "Intent",
    SUM("AHT") + SUM("ACW") AS "Total_Handle_and_Work_Time",
    SUM("Agent_Shift_Time") AS "Total_Shift_Hours",
    COUNT("Conversation_id") AS "Conversations"
  FROM 
    public."Key-insight-data-2"
    GROUP BY "Emp_ID" , "Conversation_start_Date","Channel","Intent","BU_Name") AS "A"
ON ("A"."Emp_ID" = "B"."Emp_ID" AND "A"."Conversation_start_Date" ="B"."Conversation_start_Date" AND "A"."Channel" = "B"."Channel" AND
"A"."BU_Name" = "B"."BU_Name" AND "A"."Intent" = "B"."Intent")



in this query getting ddl mml error in apache superset resoolve
