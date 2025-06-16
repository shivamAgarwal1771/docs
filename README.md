     
type Column = { column_name: string };

[
          {
            name: 'extra_tooltip_field',
            config: {
              type: 'SelectControl',
              label: t('Additional Tooltip Field'),
              default: null,
              renderTrigger: true,
              clearable: true,
              description: t('Pick a column to show in tooltip.'),
              mapStateToProps: (state: any) => {
                const columns = state?.datasource?.columns ?? [];
                const choices = Array.isArray(columns)
                  ? columns
                    .filter(col => typeof col?.column_name === 'string')
                    .map(col => [col.column_name, col.column_name])
                  : [];

                return {
                  choices,
                };
              },

            },
          }

        ],



const xValueRaw = richTooltip ? params[0].name : params.name;
        const xValue: number | null =
          typeof xValueRaw === 'number' ? xValueRaw : Number(xValueRaw);



        // 🧪 Debug info
        console.log('🔍 Tooltip xValue:', xValue);
        console.log('📊 Data sample:', data?.[0]);
        console.log('🪓 xAxisLabel:', xAxisLabel);
        console.log('🪓 xAxisOrig:', xAxisOrig);

        // ✅ Extra field logic
        const customField = formData.extraTooltipField;
        if (customField && data?.length > 0 && xValue !== null) {
          const currentX = String(xValue);

          const matchingRow = data.find(d =>
            String(d?.[xAxisLabel]) === currentX ||
            String(d?.[xAxisOrig]) === currentX
          );

          if (matchingRow && matchingRow[customField] != null) {
            const fieldLabel = customField
              .replace(/_/g, ' ')
              .replace(/\b\w/g, (l: string) => l.toUpperCase())
            rows.push([fieldLabel, String(matchingRow[customField])]);
            console.log('✅ Extra field in tooltip:', fieldLabel, matchingRow[customField]);
          } else {
            console.warn('⚠️ No matching row or missing extra field value');
          }
        }
        if (formData.custom_sql_unit) {
          (echartOptions as any).graphic = [
            {
              type: 'text',
              left: '95%',
              top: '5%',
              style: {
                text: formData.custom_sql_unit,
                fill: '#999',
                font: '14px sans-serif',
                textAlign: 'right',
              },
              silent: true,
            },
          ];
        }

