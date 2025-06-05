SELECT
    A."Emp_ID",
    A."Conversation_start_Date",
    A."Channel",
    A."BU_Name",
    A."Intent",
    A."Total_Handle_and_Work_Time",
    A."Total_Shift_Hours",
    A."Conversations",
    COUNT(DISTINCT B."Emp_ID") AS "Agents",
    COUNT(DISTINCT B."Conversation_start_Date") AS "Days"
FROM
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
    GROUP BY 
        "Emp_ID", "Conversation_start_Date", "Channel", "BU_Name", "Intent"
) AS A
LEFT JOIN public."Key-insight-data-2" AS B
    ON A."Emp_ID" = B."Emp_ID" 
    AND A."Conversation_start_Date" = B."Conversation_start_Date" 
    AND A."Channel" = B."Channel" 
    AND A."BU_Name" = B."BU_Name" 
    AND A."Intent" = B."Intent"
GROUP BY 
    A."Emp_ID",
    A."Conversation_start_Date",
    A."Channel",
    A."BU_Name",
    A."Intent",
    A."Total_Handle_and_Work_Time",
    A."Total_Shift_Hours",
    A."Conversations"
