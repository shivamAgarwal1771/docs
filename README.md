SELECT
  "BU",
  "Stage",
  -- Apply color formatting using span tag
  MAX(CASE
    WHEN "Track" = 'VA' THEN
      CASE
        WHEN "Stage_status" IN ('Completed_On Time', 'Completed_Delayed') THEN '<span style="color:green;">' || "Stage_status" || '</span>'
        WHEN "Stage_status" IN ('Ongoing_On Time', 'Ongoing_Delayed') THEN '<span style="color:orange;">' || "Stage_status" || '</span>'
        ELSE "Stage_status"
      END
  END) AS "VA",

  MAX(CASE
    WHEN "Track" = 'InsightsHub- VA' THEN
      CASE
        WHEN "Stage_status" IN ('Completed_On Time', 'Completed_Delayed') THEN '<span style="color:green;">' || "Stage_status" || '</span>'
        WHEN "Stage_status" IN ('Ongoing_On Time', 'Ongoing_Delayed') THEN '<span style="color:orange;">' || "Stage_status" || '</span>'
        ELSE "Stage_status"
      END
  END) AS "InsightsHub- VA",

  MAX(CASE
    WHEN "Track" = 'IDP' THEN
      CASE
        WHEN "Stage_status" IN ('Completed_On Time', 'Completed_Delayed') THEN '<span style="color:green;">' || "Stage_status" || '</span>'
        WHEN "Stage_status" IN ('Ongoing_On Time', 'Ongoing_Delayed') THEN '<span style="color:orange;">' || "Stage_status" || '</span>'
        ELSE "Stage_status"
      END
  END) AS "IDP",

  MAX(CASE
    WHEN "Track" = 'InsightsHub- IDP' THEN
      CASE
        WHEN "Stage_status" IN ('Completed_On Time', 'Completed_Delayed') THEN '<span style="color:green;">' || "Stage_status" || '</span>'
        WHEN "Stage_status" IN ('Ongoing_On Time', 'Ongoing_Delayed') THEN '<span style="color:orange;">' || "Stage_status" || '</span>'
        ELSE "Stage_status"
      END
  END) AS "InsightsHub- IDP"

FROM public."Tracker_data_v4"
WHERE "BU" IN ('PSaS')
GROUP BY "BU", "Stage"
ORDER BY "BU", "Stage"
LIMIT 1000
