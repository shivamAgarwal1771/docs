path-superset-frontend/plugins/plugin-chart-echarts/src/Gauge/controlPanel.tsx

changes -

      [
          {
            name: 'label_position',
            config: {
              type: 'SelectControl',
              label: 'Label Position',
              renderTrigger: true,
              default: 'bottom',
              choices: [
                ['bottom', 'Bottom'],
                ['top', 'Top'],
              ],
              description: 'Position of the dimension label relative to the gauge',
            },
          },          
        ],
        [
          {
            name: 'hideDimensionLabel',
            config: {
              type: 'CheckboxControl',
              label: t('Hide Dimension Label'),
              description: t('Hide the label of the dimension below the gauge'),
              default: false,
              renderTrigger: true,
            },
          },
        ],






path -superset-frontend/plugins/plugin-chart-echarts/src/Gauge/transformProps.ts

code -

/**
 * Licensed to the Apache Software Foundation (ASF) under one
 * or more contributor license agreements.  See the NOTICE file
 * distributed with this work for additional information
 * regarding copyright ownership.  The ASF licenses this file
 * to you under the Apache License, Version 2.0 (the
 * "License"); you may not use this file except in compliance
 * with the License.  You may obtain a copy of the License at
 *
 *   http://www.apache.org/licenses/LICENSE-2.0
 *
 * Unless required by applicable law or agreed to in writing,
 * software distributed under the License is distributed on an
 * "AS IS" BASIS, WITHOUT WARRANTIES OR CONDITIONS OF ANY
 * KIND, either express or implied.  See the License for the
 * specific language governing permissions and limitations
 * under the License.
 */
import {
  QueryFormMetric,
  CategoricalColorNamespace,
  CategoricalColorScale,
  DataRecord,
  getMetricLabel,
  getColumnLabel,
  getValueFormatter,
  tooltipHtml,
} from '@superset-ui/core';
import type { EChartsCoreOption } from 'echarts/core';
import type { GaugeSeriesOption } from 'echarts/charts';
import type { GaugeDataItemOption } from 'echarts/types/src/chart/gauge/GaugeSeries';
import type { CallbackDataParams } from 'echarts/types/src/util/types';
import { range } from 'lodash';
import { parseNumbersList } from '../utils/controls';
import {
  DEFAULT_FORM_DATA as DEFAULT_GAUGE_FORM_DATA,
  EchartsGaugeFormData,
  AxisTickLineStyle,
  GaugeChartTransformedProps,
  EchartsGaugeChartProps,
} from './types';
import {
  defaultGaugeSeriesOption,
  INTERVAL_GAUGE_SERIES_OPTION,
  OFFSETS,
  FONT_SIZE_MULTIPLIERS,
} from './constants';
import { OpacityEnum } from '../constants';
import { getDefaultTooltip } from '../utils/tooltip';
import { Refs } from '../types';
import { getColtypesMapping } from '../utils/series';

export const getIntervalBoundsAndColors = (
  intervals: string,
  intervalColorIndices: string,
  colorFn: CategoricalColorScale,
  min: number,
  max: number,
): Array<[number, string]> => {
  let intervalBoundsNonNormalized;
  let intervalColorIndicesArray;
  try {
    intervalBoundsNonNormalized = parseNumbersList(intervals, ',');
    intervalColorIndicesArray = parseNumbersList(intervalColorIndices, ',');
  } catch (error) {
    intervalBoundsNonNormalized = [] as number[];
    intervalColorIndicesArray = [] as number[];
  }

  const intervalBounds = intervalBoundsNonNormalized.map(
    bound => (bound - min) / (max - min),
  );
  const intervalColors = intervalColorIndicesArray.map(
    ind => colorFn.colors[(ind - 1) % colorFn.colors.length],
  );

  return intervalBounds.map((val, idx) => {
    const color = intervalColors[idx];
    return [val, color || colorFn.colors[idx]];
  });
};

const calculateAxisLineWidth = (
  data: DataRecord[],
  fontSize: number,
  overlap: boolean,
): number => (overlap ? fontSize : data.length * fontSize);

const calculateMin = (data: GaugeDataItemOption[]) =>
  2 * Math.min(...data.map(d => d.value as number).concat([0]));

const calculateMax = (data: GaugeDataItemOption[]) =>
  2 * Math.max(...data.map(d => d.value as number).concat([0]));

export default function transformProps(
  chartProps: EchartsGaugeChartProps,
): GaugeChartTransformedProps {
  const {
    width,
    height,
    formData,
    queriesData,
    hooks,
    filterState,
    theme,
    emitCrossFilters,
    datasource,
  } = chartProps;

  const { hideDimensionLabel = false } = formData;
  const { label_position = 'bottom' } = formData;
  const gaugeSeriesOptions = defaultGaugeSeriesOption(theme);
  const {
    verboseMap = {},
    currencyFormats = {},
    columnFormats = {},
  } = datasource;
  const {
    groupby,
    metric,
    minVal,
    maxVal,
    colorScheme,
    fontSize,
    numberFormat,
    currencyFormat,
    animation,
    showProgress,
    overlap,
    roundCap,
    showAxisTick,
    showSplitLine,
    splitNumber,
    startAngle,
    endAngle,
    showPointer,
    intervals,
    intervalColorIndices,
    valueFormatter,
    sliceId,
  }: EchartsGaugeFormData = { ...DEFAULT_GAUGE_FORM_DATA, ...formData };
  const refs: Refs = {};
  const data = (queriesData[0]?.data || []) as DataRecord[];
  const coltypeMapping = getColtypesMapping(queriesData[0]);
  const numberFormatter = getValueFormatter(
    metric,
    currencyFormats,
    columnFormats,
    numberFormat,
    currencyFormat,
  );
  const colorFn = CategoricalColorNamespace.getScale(colorScheme as string);
  const axisLineWidth = calculateAxisLineWidth(data, fontSize, overlap);
  const groupbyLabels = groupby.map(getColumnLabel);
  const formatValue = (value: number) =>
    valueFormatter.replace('{value}', numberFormatter(value));
  const axisTickLength = FONT_SIZE_MULTIPLIERS.axisTickLength * fontSize;
  const splitLineLength = FONT_SIZE_MULTIPLIERS.splitLineLength * fontSize;
  const titleOffsetFromTitle =
    FONT_SIZE_MULTIPLIERS.titleOffsetFromTitle * fontSize;
  const detailOffsetFromTitle =
    FONT_SIZE_MULTIPLIERS.detailOffsetFromTitle * fontSize;
  const columnsLabelMap = new Map<string, string[]>();
  const metricLabel = getMetricLabel(metric as QueryFormMetric);

  const transformedData: GaugeDataItemOption[] = [];

  data.forEach((data_point, index) => {
    const name = groupbyLabels
      .map(column => `${verboseMap[column] || column}: ${data_point[column]}`)
      .join(', ');
    const colorLabel = groupbyLabels.map(col => data_point[col] as string);
    columnsLabelMap.set(
      name,
      groupbyLabels.map(col => data_point[col] as string),
    );

    const baseItem: Partial<GaugeDataItemOption> = {
      value: data_point[metricLabel] as number,
      name,
      itemStyle: {
        color: colorFn(colorLabel, sliceId),
      },
      detail: {
        fontSize: FONT_SIZE_MULTIPLIERS.detailFontSize * fontSize,
      },
      title: {
        fontSize,
      },
    };

    const labelPositions: Array<'top' | 'bottom'> =
      label_position === 'both' ? ['top', 'bottom'] : [label_position as 'top' | 'bottom'];

    labelPositions.forEach((position, posIndex) => {
      const offsetMultiplier = position === 'top' ? -1 : 1;
      let item: GaugeDataItemOption = {
        ...baseItem,
        title: {
          ...baseItem.title,
          show: !hideDimensionLabel,
          offsetCenter: [
            '0%',
            `${offsetMultiplier * (index * titleOffsetFromTitle + OFFSETS.titleFromCenter)}%`,
          ],
        },
        detail: {
          ...baseItem.detail,
          offsetCenter: [
            '0%',
            `${offsetMultiplier * (
              index * titleOffsetFromTitle +
              OFFSETS.titleFromCenter +
              detailOffsetFromTitle
            )}%`,
          ],
        },
      };

      if (
        filterState.selectedValues &&
        !filterState.selectedValues.includes(name)
      ) {
        item = {
          ...item,
          itemStyle: {
            color: colorFn(index, sliceId),
            opacity: OpacityEnum.SemiTransparent,
          },
          detail: {
            ...item.detail,
            show: false,
          },
          title: {
            ...item.title,
            show: !hideDimensionLabel,
          },
        };
      }

      transformedData.push(item);
    });
  });

  const { setDataMask = () => {}, onContextMenu } = hooks;

  const min = minVal ?? calculateMin(transformedData);
  const max = maxVal ?? calculateMax(transformedData);
  const axisLabels = range(min, max, (max - min) / splitNumber);
  const axisLabelLength = Math.max(
    ...axisLabels.map(label => numberFormatter(label).length).concat([1]),
  );
  const intervalBoundsAndColors = getIntervalBoundsAndColors(
    intervals,
    intervalColorIndices,
    colorFn,
    min,
    max,
  );
  const splitLineDistance =
    axisLineWidth + splitLineLength + OFFSETS.ticksFromLine;
  const axisLabelDistance =
    FONT_SIZE_MULTIPLIERS.axisLabelDistance *
      fontSize *
      FONT_SIZE_MULTIPLIERS.axisLabelLength *
      axisLabelLength +
    (showSplitLine ? splitLineLength : 0) +
    (showAxisTick ? axisTickLength : 0) +
    OFFSETS.ticksFromLine -
    axisLineWidth;
  const axisTickDistance =
    axisLineWidth + axisTickLength + OFFSETS.ticksFromLine;

  const progress = {
    show: showProgress,
    overlap,
    roundCap,
    width: fontSize,
  };
  const splitLine = {
    show: showSplitLine,
    distance: -splitLineDistance,
    length: splitLineLength,
    lineStyle: {
      width: FONT_SIZE_MULTIPLIERS.splitLineWidth * fontSize,
      color: gaugeSeriesOptions.splitLine?.lineStyle?.color,
    },
  };
  const axisLine = {
    roundCap,
    lineStyle: {
      width: axisLineWidth,
      color: gaugeSeriesOptions.axisLine?.lineStyle?.color,
    },
  };
  const axisLabel = {
    distance: -axisLabelDistance,
    fontSize,
    formatter: numberFormatter,
    color: gaugeSeriesOptions.axisLabel?.color,
  };
  const axisTick = {
    show: showAxisTick,
    distance: -axisTickDistance,
    length: axisTickLength,
    lineStyle: gaugeSeriesOptions.axisTick?.lineStyle as AxisTickLineStyle,
  };
  const detail = {
    valueAnimation: animation,
    formatter: (value: number) => formatValue(value),
    color: gaugeSeriesOptions.detail?.color,
  };
  const tooltip = {
    ...getDefaultTooltip(refs),
    formatter: (params: CallbackDataParams) => {
      const { name, value } = params;
      return tooltipHtml([[metricLabel, formatValue(value as number)]], name);
    },
  };

  let pointer;
  if (intervalBoundsAndColors.length) {
    splitLine.lineStyle.color =
      INTERVAL_GAUGE_SERIES_OPTION.splitLine?.lineStyle?.color;
    axisTick.lineStyle.color = INTERVAL_GAUGE_SERIES_OPTION?.axisTick?.lineStyle
      ?.color as string;
    axisLabel.color = INTERVAL_GAUGE_SERIES_OPTION.axisLabel?.color;
    axisLine.lineStyle.color = intervalBoundsAndColors;
    pointer = {
      show: showPointer,
      showAbove: false,
      itemStyle: INTERVAL_GAUGE_SERIES_OPTION.pointer?.itemStyle,
    };
  } else {
    pointer = {
      show: showPointer,
      showAbove: false,
    };
  }

  const series: GaugeSeriesOption[] = [
    {
      type: 'gauge',
      startAngle,
      endAngle,
      min,
      max,
      progress,
      animation,
      axisLine: axisLine as GaugeSeriesOption['axisLine'],
      splitLine,
      splitNumber,
      axisLabel,
      axisTick,
      pointer,
      detail,
      // @ts-ignore
      tooltip,
      radius:
        Math.min(width, height) / 2 - axisLabelDistance - axisTickDistance,
      center: ['50%', '50%'],
      data: transformedData,
    },
  ];

  const echartOptions: EChartsCoreOption = {
    tooltip: {
      ...getDefaultTooltip(refs),
      trigger: 'item',
    },
    series,
  };

  return {
    formData,
    width,
    height,
    echartOptions,
    setDataMask,
    emitCrossFilters,
    labelMap: Object.fromEntries(columnsLabelMap),
    groupby,
    selectedValues: filterState.selectedValues || [],
    onContextMenu,
    refs,
    coltypeMapping,
  };
}


path-superset-frontend/plugins/plugin-chart-echarts/src/Timeseries/transformProps.ts

changes-graphic: unit
    ? [
        {
          type: 'text',
          left: '95%',
          top: '5%',
          style: {
            text: unit,
            fill: '#999',
            font: '14px sans-serif',
            textAlign: 'right',
          },
          silent: true,
        },
      ]
    : [],


path-superset-frontend/plugins/plugin-chart-echarts/src/Timeseries/Regular/Line/controlPanel.tsx

changes- [
          {
            name: 'unit',
            config: {
              type: 'TextControl',
              label: t('Unit'),
              renderTrigger: true,
              default: '',
              description: t('Text to show on top-right corner of chart as unit.'),
            },
          },
        ],


path-superset-frontend/plugins/plugin-chart-echarts/src/Treemap/controlPanel.tsx

changes-

[
          {
            name: 'treemapFont',
            config: {
              type: 'TextControl',
              label: t('Font'),
              description: t('Set the font style for labels (e.g., 14px sans-serif)'),
              default: '14px sans-serif',
              renderTrigger: true,
            },
          },
        ],       


path-superset-frontend/plugins/plugin-chart-echarts/src/Treemap/transformProps.ts

code -
/**
 * Licensed to the Apache Software Foundation (ASF) under one
 * or more contributor license agreements.  See the NOTICE file
 * distributed with this work for additional information
 * regarding copyright ownership.  The ASF licenses this file
 * to you under the Apache License, Version 2.0 (the
 * "License"); you may not use this file except in compliance
 * with the License.  You may obtain a copy of the License at
 *
 *   http://www.apache.org/licenses/LICENSE-2.0
 *
 * Unless required by applicable law or agreed to in writing,
 * software distributed under the License is distributed on an
 * "AS IS" BASIS, WITHOUT WARRANTIES OR CONDITIONS OF ANY
 * KIND, either express or implied.  See the License for the
 * specific language governing permissions and limitations
 * under the License.
 */
import {
  CategoricalColorNamespace,
  getColumnLabel,
  getMetricLabel,
  getNumberFormatter,
  getTimeFormatter,
  NumberFormats,
  ValueFormatter,
  getValueFormatter,
  tooltipHtml,
} from '@superset-ui/core';
import type { TreemapSeriesNodeItemOption } from 'echarts/types/src/chart/treemap/TreemapSeries';
import type { EChartsCoreOption } from 'echarts/core';
import type { TreemapSeriesOption } from 'echarts/charts';
import {
  DEFAULT_FORM_DATA as DEFAULT_TREEMAP_FORM_DATA,
  EchartsTreemapChartProps,
  EchartsTreemapFormData,
  EchartsTreemapLabelType,
  TreemapSeriesCallbackDataParams,
  TreemapTransformedProps,
} from './types';
import { formatSeriesName, getColtypesMapping } from '../utils/series';
import {
  COLOR_SATURATION,
  BORDER_WIDTH,
  GAP_WIDTH,
  LABEL_FONTSIZE,
  extractTreePathInfo,
  BORDER_COLOR,
} from './constants';
import { OpacityEnum } from '../constants';
import { getDefaultTooltip } from '../utils/tooltip';
import { Refs } from '../types';
import { treeBuilder, TreeNode } from '../utils/treeBuilder';

export function formatLabel({
  params,
  labelType,
  numberFormatter,
}: {
  params: TreemapSeriesCallbackDataParams;
  labelType: EchartsTreemapLabelType;
  numberFormatter: ValueFormatter;
}): string {
  const { name = '', value } = params;
  const formattedValue = numberFormatter(value as number);

  switch (labelType) {
    case EchartsTreemapLabelType.Key:
      return name;
    case EchartsTreemapLabelType.Value:
      return formattedValue;
    case EchartsTreemapLabelType.KeyValue:
      return `${name}: ${formattedValue}`;
    default:
      return name;
  }
}

export function formatTooltip({
  params,
  numberFormatter,
}: {
  params: TreemapSeriesCallbackDataParams;
  numberFormatter: ValueFormatter;
}): string {
  const { value, treePathInfo = [] } = params;
  const formattedValue = numberFormatter(value as number);
  const { metricLabel, treePath } = extractTreePathInfo(treePathInfo);
  const percentFormatter = getNumberFormatter(NumberFormats.PERCENT_2_POINT);

  let formattedPercent = '';
  const currentNode = treePathInfo[treePathInfo.length - 1];
  const parentNode = treePathInfo[treePathInfo.length - 2];
  if (parentNode) {
    const percent: number = parentNode.value
      ? (currentNode.value as number) / (parentNode.value as number)
      : 0;
    formattedPercent = percentFormatter(percent);
  }
  const row = [metricLabel, formattedValue];
  if (formattedPercent) {
    row.push(formattedPercent);
  }
  return tooltipHtml([row], treePath.join(' ▸ '));
}

export default function transformProps(
  chartProps: EchartsTreemapChartProps,
): TreemapTransformedProps {
  const {
    formData,
    height,
    queriesData,
    width,
    hooks,
    filterState,
    theme,
    inContextMenu,
    emitCrossFilters,
    datasource,
  } = chartProps;
  const { treemapFont = '14px sans-serif' } = formData;
  const fontSize = parseInt(treemapFont, 10) || 14;
  const fontFamily = treemapFont.split(' ').slice(1).join(' ') || 'sans-serif';

  const { data = [] } = queriesData[0];
  const { columnFormats = {}, currencyFormats = {} } = datasource;
  const { setDataMask = () => {}, onContextMenu } = hooks;
  const coltypeMapping = getColtypesMapping(queriesData[0]);

  const {
    colorScheme,
    groupby = [],
    metric = '',
    labelType,
    labelPosition,
    numberFormat,
    currencyFormat,
    dateFormat,
    showLabels,
    showUpperLabels,
    dashboardId,
    sliceId,
  }: EchartsTreemapFormData = {
    ...DEFAULT_TREEMAP_FORM_DATA,
    ...formData,
  };

  const refs: Refs = {};
  const colorFn = CategoricalColorNamespace.getScale(colorScheme as string);
  const numberFormatter = getValueFormatter(
    metric,
    currencyFormats,
    columnFormats,
    numberFormat,
    currencyFormat,
  );

  const formatter = (params: TreemapSeriesCallbackDataParams) =>
    formatLabel({
      params,
      numberFormatter,
      labelType,
    });

  const columnsLabelMap = new Map<string, string[]>();
  const metricLabel = getMetricLabel(metric);
  const groupbyLabels = groupby.map(getColumnLabel);
  const treeData = treeBuilder(data, groupbyLabels, metricLabel);

  const traverse = (treeNodes: TreeNode[], path: string[]) =>
    treeNodes.map(treeNode => {
      const { name: nodeName, value, groupBy } = treeNode;
      const name = formatSeriesName(nodeName, {
        timeFormatter: getTimeFormatter(dateFormat),
        ...(coltypeMapping[groupBy] && {
          coltype: coltypeMapping[groupBy],
        }),
      });
      const newPath = path.concat(name);
      let item: TreemapSeriesNodeItemOption = {
        name,
        value,
        colorSaturation: COLOR_SATURATION,
        itemStyle: {
          borderColor: BORDER_COLOR,
          color: colorFn(name, sliceId),
          borderWidth: BORDER_WIDTH,
          gapWidth: GAP_WIDTH,
        },
      };
      if (treeNode.children?.length) {
        item = {
          ...item,
          children: traverse(treeNode.children, newPath),
        };
      } else {
        const joinedName = newPath.join(',');
        columnsLabelMap.set(joinedName, newPath);
        if (
          filterState.selectedValues &&
          !filterState.selectedValues.includes(joinedName)
        ) {
          item = {
            ...item,
            itemStyle: {
              colorAlpha: OpacityEnum.SemiTransparent,
            },
            label: {
              color: `rgba(0, 0, 0, ${OpacityEnum.SemiTransparent})`,
            },
          };
        }
      }
      return item;
    });

  const transformedData: TreemapSeriesNodeItemOption[] = [
    {
      name: metricLabel,
      colorSaturation: COLOR_SATURATION,
      itemStyle: {
        borderColor: BORDER_COLOR,
        color: colorFn(`${metricLabel}`, sliceId),
        borderWidth: BORDER_WIDTH,
        gapWidth: GAP_WIDTH,
      },
      upperLabel: {
        show: false,
      },
      children: traverse(treeData, []),
    },
  ];

  const levels = [
    {
      upperLabel: {
        show: false,
      },
      label: {
        show: false,
      },
      itemStyle: {
        color: theme.colors.primary.base,
      },
    },
  ];

  const series: TreemapSeriesOption[] = [
    
    {
      type: 'treemap',
      width: '100%',
      height: '100%',
      nodeClick: undefined,
      roam: !dashboardId,
      breadcrumb: {
        show: false,
        emptyItemWidth: 25,
      },
      emphasis: {
        label: {
          show: true,
        },
      },
      levels,
      label: {
        show: showLabels,
        position: labelPosition,
        formatter,
        color: theme.colors.grayscale.dark2,
        fontSize,
        fontFamily,
      },
      upperLabel: {
        show: showUpperLabels,
        formatter,
        textBorderColor: 'transparent',
        fontSize,
        fontFamily,
      },
      data: transformedData,
    },
  ];

  const echartOptions: EChartsCoreOption = {
    tooltip: {
      ...getDefaultTooltip(refs),
      show: !inContextMenu,
      trigger: 'item',
      formatter: (params: any) =>
        formatTooltip({
          params,
          numberFormatter,
        }),
    },
    series,
  };

  return {
    formData,
    width,
    height,
    echartOptions,
    setDataMask,
    emitCrossFilters,
    labelMap: Object.fromEntries(columnsLabelMap),
    groupby,
    selectedValues: filterState.selectedValues || [],
    onContextMenu,
    refs,
    coltypeMapping,
  };
}

path-superset-frontend/src/assets/images/7.png

add all the images for dashboard on this path



path-superset-frontend/src/dashboard/components/nativeFilters/FilterBar/index.tsx

changes-

const filterBarComponent =(
        <Horizontal
        actions={actions}
        canEdit={canEdit}
        dashboardId={dashboardId}
        dataMaskSelected={dataMaskSelected}
        filterValues={filterValues}
        isInitialized={isInitialized}
        onSelectionChange={handleFilterSelectionChange}
      />
  );


path-superset-frontend/src/features/dashboards/DashboardCard.tsx

change-

      cover={
          showThumbnails && dashboard.thumbnail_url ? (
            <img
              src={`/static/assets/images/${dashboard.id}.png`}
              alt="Dashboard Thumbnail"
              style={{
                width: '100%',
                height: '150px',
                objectFit: 'cover',
                borderRadius: '4px',
              }}
            />
          ) : null
        }
        url={bulkSelectEnabled ? undefined : dashboard.url}
        linkComponent={Link}
        imgURL={dashboard.thumbnail_url}
        imgFallbackURL={assetUrl(
          `/static/assets/images/${dashboard.id}.png`,
        )}



path-superset-frontend/src/features/tags/TagCard.tsx

changes-
   imgFallbackURL={assetUrl(
          `/static/assets/images/11.png`,
        )}


path- superset-frontend/src/pages/DashboardList/index.tsx

defaultViewMode='card'



