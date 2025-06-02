📘 Superset Custom Enhancements Documentation
Last Updated: 2025-06-02 11:02:05

1.  Gauge Chart Enhancements
📁 File: src/Gauge/controlPanel.tsx
➕ Added Controls
✅ label_position
Description: Allows users to position the dimension label either above or below the gauge.

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

✅ hideDimensionLabel
Description: Enables toggling the visibility of the dimension label.

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

📁 File: src/Gauge/transformProps.ts
🧠 Logic Integration
🔀 Label Position Handling
Description: Dynamically places labels based on label_position and supports multiple entries with adjusted offsetCenter.

const { hideDimensionLabel = false } = formData;
const { label_position = 'bottom' } = formData;

// Determine label position(s)
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

  // Hide items if not selected
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

2. 📈 Timeseries Line Chart Enhancement
📁 File: src/Timeseries/Regular/Line/controlPanel.tsx
➕ unit
Description: Adds a "Unit" input field, allowing users to display a label (e.g., %, °C) at the top-right corner of the chart.

[
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

📁 File: src/Timeseries/transformProps.ts
🧠 Unit Display via graphic
Description: Renders the provided unit text at the top-right using ECharts' graphic property.

graphic: unit
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

3. 🌳 Treemap Chart Enhancement
📁 File: src/Treemap/controlPanel.tsx
➕ treemapFont
Description: Adds a text control allowing users to set a custom font style for the Treemap chart labels.

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

📷 Dashboard Thumbnails & UI Enhancements
🖼️ Path: src/assets/images/7.png
Action: Place your dashboard thumbnail images (like 7.png, 11.png, etc.) here.
🧩 File: src/dashboard/components/nativeFilters/FilterBar/index.tsx
Change: Replace or insert the filter bar component.

const filterBarComponent = (
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

🗂️ File: src/features/dashboards/DashboardCard.tsx
Change: Modify the cover prop to load dashboard-specific thumbnails.

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
imgFallbackURL={assetUrl(`/static/assets/images/${dashboard.id}.png`)}

🔖 File: src/features/tags/TagCard.tsx
Change: Default image fallback for tag cards.
imgFallbackURL={assetUrl(`/static/assets/images/11.png`)}
📄 File: src/pages/DashboardList/index.tsx
Change: Set default view mode to card.
defaultViewMode="card"
