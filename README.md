SELECT
  "BU" AS "BU",
  "Stage" AS "Stage",
  "VA" AS "VA",
  "InsightsHub- VA" AS "InsightsHub- VA",
  "IDP" AS "IDP",
  "InsightsHub- IDP" AS "InsightsHub- IDP"
FROM (
  SELECT
    "BU",
    "Stage",
    MAX(CASE WHEN "Track" = 'VA' THEN "Stage_status" END) AS "VA",
    MAX(CASE WHEN "Track" = 'InsightsHub- VA' THEN "Stage_status" END) AS "InsightsHub- VA",
    MAX(CASE WHEN "Track" = 'IDP' THEN "Stage_status" END) AS "IDP",
    MAX(CASE WHEN "Track" = 'InsightsHub- IDP' THEN "Stage_status" END) AS "InsightsHub- IDP"
  FROM public."Tracker_data_v4"
  GROUP BY
    "BU",
    "Stage"
  ORDER BY
    "BU",
    "Stage"
) AS virtual_table
WHERE
  "BU" IN ('PSaS')
LIMIT 1000







SELECT
  "BU" AS "BU",
  "Track" AS "Track",
  "Activity" AS "Activity",
  "Current_Status" AS "Current_Status",
  "Comment" AS "Comment",
  "Help_Needed" AS "Help_Needed"
FROM public."Tracker_data_v4"
WHERE
  "Status" IN ('Ongoing') AND "BU" IN ('PSaS')
ORDER BY
  "Track" DESC
LIMIT 1000






SELECT
  "BU" AS "BU",
  "Track" AS "Track",
  "Stage" AS "Stage",
  "Activity" AS "Activity",
  "Current_Status" AS "Current_Status"
FROM public."Tracker_data_v4"
WHERE
  "Last_wk_activity" IN (1) AND "BU" IN ('PSaS')
ORDER BY
  "Track" DESC,
  "Stage" ASC
LIMIT 1000






SELECT
  "BU" AS "BU",
  "Track" AS "Track",
  "Stage" AS "Stage",
  "Activity" AS "Activity",
  "Current_Status" AS "Current_Status",
  "Comment" AS "Comment",
  "Help_Needed" AS "Help_Needed"
FROM public."Tracker_data_v4"
WHERE
  "Next_wk_activity" IN (1) AND "BU" IN ('PSaS')
ORDER BY
  "Track" DESC,
  "Stage" ASC
LIMIT 1000
