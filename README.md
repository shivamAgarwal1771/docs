import {
  buildQueryContext,
  ensureIsArray,
  getXAxisColumn,
  isXAxisSet,
  normalizeOrderBy,
  PostProcessingPivot,
  QueryFormData,
  getMetricLabel,
  NumpyFunction,
} from '@superset-ui/core';
import {
  contributionOperator,
  extractExtraMetrics,
  flattenOperator,
  isTimeComparison,
  pivotOperator,
  prophetOperator,
  renameOperator,
  resampleOperator,
  rollingWindowOperator,
  sortOperator,
  timeComparePivotOperator,
  timeCompareOperator,
} from '@superset-ui/chart-controls';

export default function buildQuery(formData: QueryFormData) {
  const {
    groupby,
    extra_tooltip_field,
    custom_unit_metric
  } = formData;
const customMetric = formData.custom_unit_metric;
console.log("kaki",customMetric)

  const customUnitMetric = formData.custom_sql_unit_metric;
  let customMetricLabel: string | null = null;

  if (customUnitMetric) {
    try {
      customMetricLabel = getMetricLabel(customUnitMetric);
      console.log('✅ Custom SQL Unit Metric label:', customMetricLabel);
      console.log('🧠 Raw custom_sql_unit_metric object:', customUnitMetric);
    } catch (e) {
      console.warn('❌ Invalid custom metric provided:', customUnitMetric);
    }
  }

  return buildQueryContext(formData, baseQueryObject => {
    const extra_metrics = extractExtraMetrics(formData);
    const xAxisCols = isXAxisSet(formData)
      ? ensureIsArray(getXAxisColumn(formData))
      : [];
    const extraFields = ensureIsArray(extra_tooltip_field);

    const cleanedExtraFields = extraFields.filter(
      f => f && !groupby?.includes(f),
    );

    const cleanedMetrics = (baseQueryObject.metrics || []).filter(metric =>
      !cleanedExtraFields.includes(
        typeof metric === 'string' ? metric : getMetricLabel(metric),
      ),
    );

    const unitMetricKey = custom_unit_metric
      ? getMetricLabel(custom_unit_metric)
      : null;

    if (unitMetricKey) {
      if (!baseQueryObject.metrics) {
        baseQueryObject.metrics = [];
      }
      baseQueryObject.metrics.push(custom_unit_metric);
      console.log('📤 custom_unit_metric added to metrics:', unitMetricKey);
    }

    const columns = [
      ...xAxisCols,
      ...ensureIsArray(groupby),
      ...cleanedExtraFields,
    ];

    const time_offsets = isTimeComparison(formData, baseQueryObject)
      ? formData.time_compare
      : [];

    const pivotOperatorInRuntime: PostProcessingPivot = isTimeComparison(
      formData,
      baseQueryObject,
    )
      ? timeComparePivotOperator(formData, baseQueryObject)
      : {
          operation: 'pivot',
          options: {
            index: xAxisCols
              .map(col => (typeof col === 'string' ? col : col.label))
              .filter((val): val is string => Boolean(val)),
            columns: ensureIsArray(groupby)
              .map(col => (typeof col === 'string' ? col : col.label))
              .filter((val): val is string => Boolean(val)),
            drop_missing_columns: false,
            aggregates: (() => {
              const allFields = [
                ...cleanedMetrics,
                ...extra_metrics,
                ...cleanedExtraFields,
              ];
              return Object.fromEntries(
                allFields.map(field => {
                  const label = typeof field === 'string' ? field : getMetricLabel(field);
                  const isTooltipField = cleanedExtraFields.includes(label);
                  const operator: NumpyFunction = isTooltipField ? 'min' : 'mean';
                  return [label, { operator }];
                }),
              );
            })(),
          },
        };

    // ✅ Attach extra fields for transformProps
    formData.extra_tooltip_field_cleaned = cleanedExtraFields;
    formData.custom_unit_sql = custom_unit_metric;
const secondQuery = customMetric
  ? {
      ...baseQueryObject,
      metrics: [customMetric],
      columns: [],
      groupby: [],
      post_processing: [],
      is_timeseries: false,
    }
  : null;

  console.log("pari",secondQuery)
    const mainQuery = {
      ...baseQueryObject,
      metrics: [...cleanedMetrics, ...extra_metrics],
      columns,
      custom_unit_metric,
      series_columns: groupby,
      ...(isXAxisSet(formData) ? {} : { is_timeseries: true }),
      orderby: normalizeOrderBy(baseQueryObject).orderby,
      time_offsets,
      post_processing: [
        pivotOperatorInRuntime,
        rollingWindowOperator(formData, baseQueryObject),
        timeCompareOperator(formData, baseQueryObject),
        resampleOperator(formData, baseQueryObject),
        renameOperator(formData, baseQueryObject),
        contributionOperator(formData, baseQueryObject, time_offsets),
        sortOperator(formData, baseQueryObject),
        flattenOperator(formData, baseQueryObject),
        prophetOperator(formData, baseQueryObject),
      ],
    };
    return [mainQuery, ...(secondQuery ? [secondQuery] : [])];
  });
}
