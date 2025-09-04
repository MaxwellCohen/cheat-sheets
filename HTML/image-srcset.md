
# Image Srcset logic for picking an image


```mermaid
flowchart TD
    A["`Start: **img** tag encountered`"] --> B{Is srcset present?}
    B -- No --> C[Use src attribute as image source]
    B -- Yes --> D["Are width descriptors (w) or pixel density descriptors  used in srcset?
    "]
    D -- Width (w) --> E[Evaluate sizes attribute or default to 100vw]
    E --> F[Calculate display size in CSS pixels]
    F --> G[Choose best candidate from srcset based on display size and device pixel ratio]
    G --> H[Set chosen image as source]
    D -- Pixel density (x) --> I[Choose best candidate from srcset based on device pixel ratio]
    I --> H
    H --> J[Display chosen image]
    C --> J
  ```
