title: {
  show: !hideDimensionLabel,
  offsetCenter: [
    '0%',
    `${
      (label_position === 'top' ? -1 : 1) * (
        index * titleOffsetFromTitle + OFFSETS.titleFromCenter
      )
    }%`,
  ],
  fontSize,
},
detail: {
  offsetCenter: [
    '0%',
    `${
      (label_position === 'top' ? -1 : 1) * (
        index * titleOffsetFromTitle +
        OFFSETS.titleFromCenter +
        detailOffsetFromTitle
      )
    }%`,
  ],
  fontSize: FONT_SIZE_MULTIPLIERS.detailFontSize * fontSize,
},
