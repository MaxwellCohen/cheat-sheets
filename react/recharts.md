# Recharts

[Recharts](https://recharts.github.io/) is a composable charting library for React: you assemble charts from small components (axes, grid, series, tooltip) and pass an array of row objects as `data`. Charts render to SVG with a light dependency on D3 submodules. Official reference: [Guide](https://recharts.github.io/en-US/guide/getting-started/), [API](https://recharts.github.io/en-US/api/), [Examples](https://recharts.github.io/en-US/examples/).

[shadcn/ui Charts](https://ui.shadcn.com/charts) are ready-made chart layouts built on Recharts (area, bar, line, pie, radar, radial, tooltips). Use them when you want themed, copy-paste components that match shadcn design tokens; the underlying primitives are still Recharts.

## Install

```bash
npm install recharts
```

```tsx
import {
  BarChart,
  Bar,
  XAxis,
  YAxis,
  CartesianGrid,
  Tooltip,
  Legend,
  ResponsiveContainer,
} from 'recharts';
```

## Mental model

1. **Data** — An array of plain objects (`data={rows}`). Each series picks fields with **`dataKey`** (string path or function).
2. **Chart** — A *chart type* component (`BarChart`, `LineChart`, …) holds `data` and layout; **children** define axes, grid, and series.
3. **Size** — Either set **`width` / `height`** on the chart, or wrap in **`ResponsiveContainer`** (or use chart `responsive` + CSS sizing where supported—see [chart sizing](https://recharts.github.io/en-US/guide/sizes/)).
4. **Cartesian vs polar** — Most charts use **`XAxis` / `YAxis`** and **`CartesianGrid`**. Radar / radial charts use **`PolarGrid`**, **`PolarAngleAxis`**, **`PolarRadiusAxis`**, etc.

## Quick start (bar)

```tsx
const data = [
  { name: 'Jan', sales: 400 },
  { name: 'Feb', sales: 300 },
];

<ResponsiveContainer width="100%" height={300}>
  <BarChart data={data} margin={{ top: 8, right: 8, left: 8, bottom: 8 }}>
    <CartesianGrid strokeDasharray="3 3" />
    <XAxis dataKey="name" />
    <YAxis />
    <Tooltip />
    <Legend />
    <Bar dataKey="sales" fill="var(--color-chart-1)" />
  </BarChart>
</ResponsiveContainer>
```

## Chart type → usual children

All **root chart** components exported from `recharts` (current [public API](https://recharts.github.io/en-US/api/)):

| Chart | Primary series / shapes | Axes / frame |
| ----- | ------------------------ | ------------ |
| [`AreaChart`](https://recharts.github.io/en-US/api/AreaChart/) | `Area` (often `stackId` for stacks); also `Line`, `Bar` | Cartesian† |
| [`BarChart`](https://recharts.github.io/en-US/api/BarChart/) | `Bar`; `BarStack` for grouped stacks | Cartesian† |
| [`LineChart`](https://recharts.github.io/en-US/api/LineChart/) | `Line`; `ErrorBar` on series | Cartesian† |
| [`ComposedChart`](https://recharts.github.io/en-US/api/ComposedChart/) | Mix `Bar`, `Line`, `Area`, `Scatter` | Cartesian† |
| [`ScatterChart`](https://recharts.github.io/en-US/api/ScatterChart/) | `Scatter`; `ErrorBar` on series | Cartesian† + optional `ZAxis` (bubble size / third measure) |
| [`FunnelChart`](https://recharts.github.io/en-US/api/FunnelChart/) | `Funnel` (trapezoids) | Cartesian† |
| [`PieChart`](https://recharts.github.io/en-US/api/PieChart/) | `Pie`, `Cell`; `Label` / `LabelList` | No axes — layout on `Pie` (`cx`, `cy`, `innerRadius`, `outerRadius`) |
| [`RadarChart`](https://recharts.github.io/en-US/api/RadarChart/) | `Radar` | `PolarGrid`, `PolarAngleAxis`, `PolarRadiusAxis` |
| [`RadialBarChart`](https://recharts.github.io/en-US/api/RadialBarChart/) | `RadialBar` | `PolarGrid`, `PolarAngleAxis`, `PolarRadiusAxis` |
| [`Treemap`](https://recharts.github.io/en-US/api/Treemap/) | Nested rectangles from tree `data`; optional `content` for custom cells | `Tooltip`; no axes (`type` `flat` \| `nest`) |
| [`Sankey`](https://recharts.github.io/en-US/api/Sankey/) | Flow diagram — `data={{ nodes, links }}`; custom `node` / `link` | `Tooltip`; no axes |
| [`SunburstChart`](https://recharts.github.io/en-US/api/SunburstChart/) | Concentric rings from hierarchical `data` | `Tooltip`; polar layout via chart props (`innerRadius`, `outerRadius`, …) |

† **Cartesian** — `XAxis`, `YAxis`, `CartesianGrid`, `Tooltip`, `Legend`; often `Brush`, `ReferenceLine`, `ReferenceArea`, `ReferenceDot`. Props for each component: [API index](https://recharts.github.io/en-US/api/).

## Building blocks (cartesian)

| Piece | Role |
| ----- | ---- |
| `ResponsiveContainer` | Fills parent; passes measured size to child chart. |
| `CartesianGrid` | Background grid; `strokeDasharray`, `vertical` / `horizontal`. |
| `XAxis` / `YAxis` | Ticks and labels; `dataKey` for category axis, `type="number"` / `"category"`, `tickFormatter`, `domain`. |
| `Tooltip` | Hover payload; customize `content`, `formatter`, `labelFormatter`. |
| `Legend` | Series legend; `layout`, `verticalAlign`, `align`. |
| `Brush` | Range selector on an axis. |
| `ReferenceLine` / `ReferenceArea` / `ReferenceDot` | Annotations. |

## Series props you use constantly

| Prop | Meaning |
| ---- | ------- |
| `dataKey` | Field in each row (or accessor) for this series. |
| `name` | Label in `Legend` / `Tooltip`. |
| `stroke` / `fill` | Line and area colors. |
| `stackId` | Group bars/areas into stacks sharing the same stack. |
| `type` (e.g. on `Line` / `Area`) | `"monotone"`, `"linear"`, `"step"`, … |

## Pie / donut

```tsx
<PieChart>
  <Pie
    data={data}
    dataKey="value"
    nameKey="name"
    cx="50%"
    cy="50%"
    innerRadius={60}
    outerRadius={80}
    paddingAngle={2}
  >
    {data.map((_, i) => (
      <Cell key={i} fill={COLORS[i % COLORS.length]} />
    ))}
  </Pie>
  <Tooltip />
</PieChart>
```

## Radar (polar)

Matches the [shadcn radar examples](https://ui.shadcn.com/charts/radar#charts): same Recharts building blocks, with shadcn wrapping layout and theme.

```tsx
<RadarChart data={data} cx="50%" cy="50%" outerRadius="80%">
  <PolarGrid />
  <PolarAngleAxis dataKey="subject" />
  <PolarRadiusAxis />
  <Radar name="A" dataKey="A" stroke="#8884d8" fill="#8884d8" fillOpacity={0.6} />
  <Tooltip />
</RadarChart>
```

## Layout and margins

- **`layout`** on cartesian charts: `"horizontal"` (default) or `"vertical"` (bars grow horizontally).
- **`margin`**: `{ top, right, bottom, left }` — leave room for ticks and legend.
- **`syncId`** on multiple charts: shared tooltip/brush behavior across charts with the same id.

## Interactivity

Charts expose pointer handlers (e.g. `onClick`, `onMouseMove` on the chart) for custom behavior. Tooltip content can be a custom React component via **`content={<MyTooltip />}`**. See [custom events](https://recharts.github.io/en-US/examples/AreaChartWithCustomEvents/).

## Accessibility

Charts support an accessibility layer (see `accessibilityLayer` and related props in the [API docs](https://recharts.github.io/en-US/api/)). Prefer meaningful `name` on series and readable tick labels.

## shadcn/ui workflow

1. Add the **Chart** component from shadcn (wraps `ResponsiveContainer` + theme CSS variables).
2. Copy a block from [Charts](https://ui.shadcn.com/charts) (e.g. [Radar](https://ui.shadcn.com/charts/radar#charts)) and adjust `data` and `dataKey`s.
3. Style via CSS variables (`--chart-1`, …) and your design system rather than hard-coding hex in every `Bar`/`Line`.

## Troubleshooting

| Issue | What to check |
| ----- | ------------- |
| Chart has zero size | Parent height/width; `ResponsiveContainer` needs a sized parent. |
| Axis shows `[object Object]` | `dataKey` must resolve to a primitive; flatten nested objects or use a function `dataKey`. |
| Tooltip empty | Series `name`, `dataKey`, and `data` rows aligned; category axis `dataKey` set if needed. |
| Stacks wrong order | `stackId` strings match; `reverseStackOrder` on chart if layering looks inverted. |

