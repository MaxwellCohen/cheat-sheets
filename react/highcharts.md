# Highcharts + React

[Highcharts](https://www.highcharts.com/) is a mature charting library: you pass a single **`options`** object (title, axes, series, tooltip, legend, …) and Highcharts renders to SVG (or canvas with modules). The official React wrapper is **[highcharts-react-official](https://www.npmjs.com/package/highcharts-react-official)** — it mounts a chart and syncs prop changes via **`chart.update()`**. Full option reference: [Highcharts Core JS API](https://api.highcharts.com/highcharts/). Note: **v4** of the React integration lives at [@highcharts/react](https://www.npmjs.com/package/@highcharts/react); this sheet targets **`highcharts-react-official`** (widely used on npm).

## Install

```bash
npm install highcharts highcharts-react-official
```

```tsx
import Highcharts from 'highcharts';
import HighchartsReact from 'highcharts-react-official';
```

## Mental model

1. **`options`** — One declarative config object; shape matches the [options reference](https://api.highcharts.com/highcharts/) (`chart`, `title`, `xAxis`, `yAxis`, `series`, `tooltip`, `legend`, `plotOptions`, …).
2. **`HighchartsReact`** — Pass **`highcharts={Highcharts}`** (after any modules are initialized) and **`options={...}`**. On re-renders, the wrapper updates the existing chart instead of remounting (unless you opt into **`immutable`**).
3. **Constructors** — Default is **`chart`**. Use **`stockChart`**, **`mapChart`**, or **`ganttChart`** via **`constructorType`** when you import the matching Highcharts bundle or init modules.
4. **Data** — Highcharts may **mutate** arrays you pass in for performance. Pass **`[...data]`** or **`data.map(...)`** if you need immutable React state. See [FAQ on mutations](https://www.npmjs.com/package/highcharts-react-official).

## Minimal line chart

```tsx
import Highcharts from 'highcharts';
import HighchartsReact from 'highcharts-react-official';

const options: Highcharts.Options = {
  title: { text: 'Sales' },
  xAxis: { categories: ['Jan', 'Feb', 'Mar'] },
  yAxis: { title: { text: 'Units' } },
  series: [{ type: 'line', name: 'Q1', data: [14, 22, 18] }],
};

export function LineChart() {
  return (
    <HighchartsReact
      highcharts={Highcharts}
      options={options}
      containerProps={{ style: { width: '100%', height: 360 } }}
    />
  );
}
```

### `Highcharts.Options` — top-level keys

In TypeScript, **`Highcharts.Options`** is the shape of the object you pass to **`options={...}`**. It is **not** a flat list of every knob: each key below is an **object** (or array) with its own subtree in the [Core options reference](https://api.highcharts.com/highcharts/). Stock, Maps, and Gantt bundles add more top-level keys (for example **`rangeSelector`** on Stock).

| Option | Type | Role | Example (add to `options` or `setOptions` as noted) |
| ------ | ---- | ---- | --------------------------------------------------- |
| [`accessibility`](https://api.highcharts.com/highcharts/accessibility) | object | Screen reader, keyboard, ARIA — requires **`modules/accessibility`**. | `accessibility: { enabled: true, keyboardNavigation: { enabled: true } }` |
| [`annotations`](https://api.highcharts.com/highcharts/annotations) | object \| array | Labels and shapes tied to points, axes, or pixels — requires **`modules/annotations`**. | `annotations: [{ labels: [{ point: { x: 3, y: 14, xAxis: 0, yAxis: 0 }, text: 'Peak' }] }]` |
| [`boost`](https://api.highcharts.com/highcharts/boost) | object | WebGL rendering for large datasets — requires **`modules/boost`**. | `boost: { useGPUTranslations: true, seriesThreshold: 50 }` |
| [`caption`](https://api.highcharts.com/highcharts/caption) | object | Text below the chart (included in exports). | `caption: { text: 'Source: internal', align: 'left' }` |
| [`chart`](https://api.highcharts.com/highcharts/chart) | object | Overall chart: **`type`**, size, **`zoomType`**, **`styledMode`**, animation, events, … | `chart: { type: 'line', height: 400, zoomType: 'x', backgroundColor: '#f8f8f8' }` |
| [`colorAxis`](https://api.highcharts.com/highcharts/colorAxis) | object \| array | Gradient or class-based color scale (heatmaps, treemaps, …). | `colorAxis: { min: 0, max: 100, stops: [[0, '#e0f7fa'], [1, '#006064']] }` |
| [`colors`](https://api.highcharts.com/highcharts/colors) | array | Default series palette; cycles when there are more series than colors. | `colors: ['#7cb5ec', '#434348', '#90ed7d', '#f7a35c']` |
| [`credits`](https://api.highcharts.com/highcharts/credits) | object | Footer credit link — often disabled for production. | `credits: { enabled: false }` or `credits: { text: 'Acme Inc.', href: 'https://example.com' }` |
| [`data`](https://api.highcharts.com/highcharts/data) | object | CSV / HTML table / spreadsheet parsing — requires **`modules/data`**. | `data: { csv: 'Month,Sales\nJan,10\nFeb,20' }` (often with `seriesMapping`) |
| [`defs`](https://api.highcharts.com/highcharts/defs) | object | Reusable SVG markers (e.g. arrowheads for annotations). | `defs: { arrow: { id: 'arrow', tagName: 'marker', refX: 5, refY: 5, markerWidth: 10, markerHeight: 10, children: [{ tagName: 'path', attrs: { d: 'M 0 0 L 10 5 L 0 10 Z' } }] } }` |
| [`drilldown`](https://api.highcharts.com/highcharts/drilldown) | object | Click to load finer-grained series — requires **`modules/drilldown`**. | `drilldown: { series: [{ id: 'regions', data: [['north', 1], ['south', 2]] }] }` (points use `drilldown: 'regions'`) |
| [`exporting`](https://api.highcharts.com/highcharts/exporting) | object | Export menu and image/PDF output — requires **`modules/exporting`**. | `exporting: { enabled: true, filename: 'chart' }` |
| [`global`](https://api.highcharts.com/highcharts/global) | object | Intended for **`Highcharts.setOptions`**, not the per-chart `options` object. | `Highcharts.setOptions({ global: { buttonTheme: { fill: '#d0d0d0' } } })` (see [global](https://api.highcharts.com/highcharts/global)) |
| [`lang`](https://api.highcharts.com/highcharts/lang) | object | Locale strings; usually set once with **`setOptions`**. | `Highcharts.setOptions({ lang: { thousandsSep: ' ', numericSymbols: ['k', 'M', 'G'] } })` |
| [`legend`](https://api.highcharts.com/highcharts/legend) | object | Legend layout and styling. | `legend: { layout: 'vertical', align: 'right', verticalAlign: 'middle' }` |
| [`loading`](https://api.highcharts.com/highcharts/loading) | object | Style of the overlay when you call **`chart.showLoading('…')`**. | `loading: { labelStyle: { fontWeight: 'bold' }, style: { backgroundColor: 'rgba(255,255,255,0.8)' } }` |
| [`navigation`](https://api.highcharts.com/highcharts/navigation) | object | Theme for exporting (and Stock Tools) buttons. | `navigation: { buttonOptions: { theme: { fill: '#f0f0f0' } } }` |
| [`noData`](https://api.highcharts.com/highcharts/noData) | object | Empty-state message — requires **`no-data-to-display`** module. | `noData: { style: { fontWeight: 'bold', fontSize: '16px' } }` (text from **`lang.noData`**) |
| [`pane`](https://api.highcharts.com/highcharts/pane) | object \| array | Polar / gauge background — needs **`highcharts-more`**. | `pane: { center: ['50%', '50%'], size: '80%', startAngle: 0, endAngle: 360 }` |
| [`plotOptions`](https://api.highcharts.com/highcharts/plotOptions) | object | Defaults for all series or per type (`line`, `column`, …). | `plotOptions: { series: { animation: { duration: 500 } }, column: { borderRadius: 3, stacking: 'normal' } }` |
| [`responsive`](https://api.highcharts.com/highcharts/responsive) | object | Extra **`chartOptions`** at max-width / max-height breakpoints. | `responsive: { rules: [{ condition: { maxWidth: 500 }, chartOptions: { legend: { align: 'center', verticalAlign: 'bottom' } } }] }` |
| [`series`](https://api.highcharts.com/highcharts/series) | array | Data and per-series overrides (highest precedence vs **`plotOptions`**). | `series: [{ type: 'line', name: 'A', data: [1, 2, 3] }, { type: 'line', name: 'B', data: [3, 2, 1] }]` |
| [`sonification`](https://api.highcharts.com/highcharts/sonification) | object | Audio charting — requires **`modules/sonification`**. | `sonification: { duration: 5000, showTooltip: false }` |
| [`subtitle`](https://api.highcharts.com/highcharts/subtitle) | object | Subtitle under the main title. | `subtitle: { text: 'FY2024', align: 'center' }` |
| [`time`](https://api.highcharts.com/highcharts/time) | object | Timezone and UTC behavior for **`datetime`** axes and tooltips. | `time: { timezone: 'America/New_York', useUTC: false }` |
| [`title`](https://api.highcharts.com/highcharts/title) | object | Main title. | `title: { text: 'Quarterly revenue', align: 'center' }` |
| [`tooltip`](https://api.highcharts.com/highcharts/tooltip) | object | Hover tooltip behavior and format. | `tooltip: { shared: true, valueDecimals: 2, headerFormat: '<b>{point.key}</b><br/>' }` |
| [`xAxis`](https://api.highcharts.com/highcharts/xAxis) | object \| array | Category, linear, datetime, …; use an array for multiple x-axes. | `xAxis: { categories: ['Q1', 'Q2', 'Q3'], crosshair: true }` |
| [`yAxis`](https://api.highcharts.com/highcharts/yAxis) | object \| array | Value axis(es); array when pairing series to different axes. | `yAxis: { title: { text: 'Units' }, min: 0, maxPadding: 0.05 }` |
| [`zAxis`](https://api.highcharts.com/highcharts/zAxis) | object \| array | Depth axis for **3D** charts (`chart.options3d.enabled`). | `zAxis: { min: 0, max: 10, title: { text: 'Depth' } }` |

Treat the **Example** column as copy-paste **fragments**: spread them into your `options` object (e.g. `const options: Highcharts.Options = { title: …, …rest }`) except where the row says **`Highcharts.setOptions`**, which applies to all charts on the page. Module-backed options only work after the matching **`import … from 'highcharts/modules/…'`** (and `module(Highcharts)` where required).

The minimal example above only sets **`title`**, **`xAxis`**, **`yAxis`**, and **`series`**; everything else uses library defaults until you add keys from this table.

## `HighchartsReact` props

| Prop | Required | Default | Role |
| ---- | -------- | ------- | ---- |
| `highcharts` | yes* | — | Highcharts namespace after modules are applied. *Wrapper may fall back to `window.Highcharts` if omitted (avoid relying on that). |
| `options` | yes | — | Chart config; see [API options](https://api.highcharts.com/highcharts/). |
| `constructorType` | no | `'chart'` | `'chart'` \| `'stockChart'` \| `'mapChart'` \| `'ganttChart'`. |
| `allowChartUpdate` | no | `true` | Set `false` to skip `chart.update()` when parent re-renders. |
| `immutable` | no | `false` | If `true`, destroys and recreates the chart on options change (slower; use when update is not enough). |
| `updateArgs` | no | `[true, true, true]` | Passed to `chart.update(redraw, oneToOne, animation)` — see [Chart#update](https://api.highcharts.com/class-reference/Highcharts.Chart#update). |
| `containerProps` | no | — | Props for the wrapper DOM node (`className`, `style`, `aria-*`, …). |
| `callback` | no | — | `(chart) => void` when the chart is created; `this` is the chart. |

## Column chart with shared options

```tsx
const options: Highcharts.Options = {
  chart: { type: 'column' },
  title: { text: 'By region' },
  xAxis: { categories: ['NA', 'EU', 'APAC'] },
  yAxis: { min: 0, title: { text: 'Revenue' } },
  legend: { layout: 'horizontal', align: 'center', verticalAlign: 'bottom' },
  tooltip: { shared: true },
  plotOptions: {
    column: { stacking: 'normal', borderRadius: 2 },
  },
  series: [
    { name: '2024', data: [48, 35, 29] },
    { name: '2025', data: [52, 41, 33] },
  ],
};

<HighchartsReact highcharts={Highcharts} options={options} />
```

## Responsive container (CSS + reflow)

Highcharts reflows when the container resizes if **`chart.reflow`** behavior is enabled (default in many setups). Size the wrapper with CSS:

```tsx
<HighchartsReact
  highcharts={Highcharts}
  options={{
    chart: { height: 320, spacing: [16, 16, 16, 16] },
    // ...
  }}
  containerProps={{ className: 'w-full min-h-[320px]' }}
/>
```

For explicit control, keep a ref to the chart and call **`chart.reflow()`** after layout changes (resize observers, sidebars).

## Updating from React state (hooks)

Keep **`options` in state** (or `useMemo` from props) so updates flow through `chart.update`:

```tsx
import { useMemo, useState } from 'react';
import Highcharts from 'highcharts';
import HighchartsReact from 'highcharts-react-official';

export function LiveChart() {
  const [points, setPoints] = useState([1, 3, 2, 4]);

  const options = useMemo<Highcharts.Options>(
    () => ({
      title: { text: 'Random walk' },
      series: [{ type: 'line', data: [...points] }], // copy → avoid HC mutating state
    }),
    [points],
  );

  return (
    <div>
      <HighchartsReact highcharts={Highcharts} options={options} />
      <button type="button" onClick={() => setPoints((p) => [...p, Math.random() * 5])}>
        Add point
      </button>
    </div>
  );
}
```

## Chart instance (`ref` / `callback`)

```tsx
import { useEffect, useRef } from 'react';
import Highcharts from 'highcharts';
import HighchartsReact from 'highcharts-react-official';

export function ChartWithRef() {
  const ref = useRef<HighchartsReact.RefObject>(null);

  useEffect(() => {
    const chart = ref.current?.chart;
    if (!chart) return;
    // chart.addSeries(...), chart.exporting, etc.
  }, []);

  return (
    <HighchartsReact
      ref={ref}
      highcharts={Highcharts}
      options={{ series: [{ data: [1, 2, 3] }] }}
      callback={(chart) => {
        // Alternative: chart here is the instance (watch for export clones)
        if (!chart.options.chart?.forExport) {
          /* store or configure */
        }
      }}
    />
  );
}
```

Type-only imports (v3.2.1+): `HighchartsReactRefObject`, `HighchartsReactProps` from `highcharts-react-official`.

## Modules (exporting, accessibility, …)

Initialize modules **once** before rendering. With **Next.js**, guard so modules run when `Highcharts` is already an object (SSR quirks):

```tsx
import Highcharts from 'highcharts';
import Exporting from 'highcharts/modules/exporting';
import Accessibility from 'highcharts/modules/accessibility';

if (typeof Highcharts === 'object') {
  Exporting(Highcharts);
  Accessibility(Highcharts);
}

// For Highcharts < 12, some module defaults are functions — call `module(Highcharts)` when `typeof module === 'function'`.
```

Then enable in **`options`**, e.g. **`exporting: { enabled: true }`**, **`accessibility: { enabled: true }`**.

## Stock chart (`constructorType`)

```tsx
import Highcharts from 'highcharts/highstock';
import HighchartsReact from 'highcharts-react-official';

const options: Highcharts.Options = {
  rangeSelector: { selected: 1 },
  series: [{ type: 'line', data: [1, 2, 3, 5, 8] }],
};

<HighchartsReact
  highcharts={Highcharts}
  constructorType="stockChart"
  options={options}
/>
```

## Useful option entry points (Core)

| Area | Options keys | Notes |
| ---- | ------------ | ----- |
| Canvas | `chart.type`, `chart.backgroundColor`, `chart.style` | `chart.zoomType`, `panning` for interaction. |
| Axes | `xAxis`, `yAxis` (arrays for multiple) | `categories`, `type: 'datetime'`, `plotLines`, `crosshair`. |
| Series | `series: [{ type, name, data, ... }]` | Per-type defaults under **`plotOptions.[seriesType]`**. |
| Tooltip | `tooltip` | `shared`, `formatter` (function returning HTML string), `valueDecimals`. |
| Legend | `legend` | `layout`, `align`, `verticalAlign`, `itemStyle`. |
| Colors | `colors` | Global palette; series override with `color`. |
| Credits | `credits` | `enabled: false` to hide. |
| No data | `noData` | Message when `series` is empty. |

Drill into each on [api.highcharts.com/highcharts](https://api.highcharts.com/highcharts/).

## Pitfalls

- **Mutating `options`** — Prefer replacing `options` or merging immutably; partial deep merges can fight `chart.update` expectations.
- **Stale closures in formatters** — `formatter` functions close over render values; wrap in `useCallback` / read from refs if they must stay fresh without full option replacement.
- **`allowChartUpdate={false}`** — Use when you drive the chart only via **`ref.current.chart`** imperative APIs.

## Links

- [highcharts-react-official (npm)](https://www.npmjs.com/package/highcharts-react-official)
- [Highcharts options reference](https://api.highcharts.com/highcharts/)
- [Chart class reference](https://api.highcharts.com/class-reference/Highcharts.Chart)
- [Highcharts React repo / demos](https://github.com/highcharts/highcharts-react)
