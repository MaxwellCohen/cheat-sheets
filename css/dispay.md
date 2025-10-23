

```mermaid
graph TB;

    A0["` display properties`"]

    A["` hiding content`"]
    B["` inline content`"]
    C["` block content`"]

    A0 --> A
    A0 --> B
    A0 --> C


    A --> A1["` <code>display:none</code>  
    Element is hidden and removed from document flow; takes no space.`"]
    A --> A2["` <code>display:contents</code>
    Element is invisible; children participate as if they were direct children of parent.
    `"]

    B --> B1["` <code>display:inline</code>
    Element only takes necessary width; flows within text. 
    `"]
    B1 --> B11["` <code>display:inline-block</code>
    Hybrid of inline and block; flows inline but respects width/height properties.`"]
    B --> B2["` children modifiers`"]
    B2 --> B21["` <code>display:inline-flex</code>
    Flexbox container that flows inline (like inline-block) but uses flexbox for children
    `"]
    B2 --> B22["` <code>display:inline-grid</code>
    Grid container that flows inline (like inline-block) but uses grid for children.
    `"]

    C --> C1["` display:block
    Element takes full width; starts on new line.
    `"]
    C --> C2["` children modifiers`"]
    C2 --> C21["` <code>display:flex</code>
    Enables flexbox layout; children become flex items for flexible spacing/alignment.
    `"]
    C2 --> C22["` <code>display:grid</code>
    Enables CSS Grid layout; creates 2D grid structure for precise child positioning.
    `"]



```



<!-- 

table	Element behaves like <table>; useful for tabular layouts.
table-row	Element behaves like <tr>; must be inside display: table.
table-cell	Element behaves like <td>; must be inside display: table-row.

list-item	Element behaves like <li>; typically includes bullets or numbers. -->