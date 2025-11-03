# Display Properties

CSS has severals ways to control the display of elements. 

```mermaid
kanban
    column1["browser controls everything"]
        task1["display:none"]
        task2["display:contents"]
    column2a[" "]
        task4["display:inline"]
    column2[" "]
        task5["display:inline-block"]
    column3[" "]
        task6["display:inline-flex"]
        task7["display:inline-grid"]
    column4[" "]
        task3["display:block"]
    column5["you control everything"]
        task8["display:flex"]
        task9["display:grid"]

```




## What do you want to do with content? 

```mermaid
graph TB;
    A0["` How do you want to display the content?`"]
    A["` Do you hide or  hide/ignore some or all of the elements you working with `"]
    A1["Use display:none or display:contents"]
    B["` Do you want the element next to each other on the same 'line' `"]
    b1["`Use 'inline' display properties`"]
    C["` use 'block' display properties`"]
    A0 --> A
    A -- yes --> A1
    A -- no --> B
    B -- yes --> b1
    B -- no --> C
```


## Hide or Ignore Elements
```mermaid
graph TB;
    A["` What do you want to hide/ignore?`"]
       
    A -- "`I don't want to see this element or its children`" --> A11["` Use <code>display:none</code>  
    Element is hidden and removed from document flow; takes no space.`"]

    A -- "This element is for grouping content and should be ignored by the browser" --> A21["` Use <code>display:contents</code>
    Element is invisible; children participate as if they were direct children of parent. 
    `"]
```
>[!WARNING]
> Don not set Interactive elements like inputs, buttons, or links to `display:contents` because they will will not be able to be used by your users.

## Inline Elements

```mermaid
graph TB;
    B["` inline content`"]
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
```


## Block Elements

```mermaid
graph TB;

     C["` block content`"]
        
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

