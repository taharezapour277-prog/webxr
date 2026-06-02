# App Evolution: Vanilla JavaScript to Modern React & Three.js Rebuild

This document outlines the evolutionary leap of the **3D Signage Customizer & Simulator** application. It compares the legacy, browser-native mockup with the modern, high-performance, full-stack React and robust raw Three.js application.

---

## 📊 Evolutionary Comparison at a Glance

| Architectural Dimension | Legacy Implementation (Previous) | Modern Implementation (Current) |
| :--- | :--- | :--- |
| **Framework & Language** | Vanilla HTML5, ES6 JavaScript, and plain CSS | **React 18**, **TypeScript 5**, and **Vite** tooling |
| **3D Rendering Approach** | Google `<model-viewer>` custom web components | Low-level **Raw Three.js WebGL 2.0 Scene Pipeline** |
| **Material & Shader Control** | High-level properties on `<model-viewer>` | Full procedural PBR material binding & custom texture map resolution (Normal, Physical Roughness, Metallics) |
| **Lighting Environment** | Basic environment map image preset, fixed lighting | Fully active studio HDR reflections (`RGBELoader`), cast dynamic soft shadow maps (`PCFSoftShadowMap`), active Point/Spotlights |
| **Responsive 3D Space** | CSS aspect-ratios, default web component resize | Bounding-box scale-fitting algorithm with fluid ResizeObserver ensuring exact **80% workspace vertical/horizontal occupancy** |
| **Failure Safety / Fallback** | Indefinite loader spinner on file load failure | **Procedural 3D Shape Constructor** rendering custom geometry fallbacks (Rectangles, Capsules, Shields, Diamonds, Hexagons) |
| **Multi-Angle Export** | Raw camera orbits, canvas screenshots outputted to page | **`three:capture-all-angles`** Event dispatcher executing high-contrast transparent PNG downloads |
| **Styling Paradigm** | Fixed external custom stylesheets | **Tailwind CSS Utility Engine** with backdrop filters, vibrant neon glows, and custom CSS page keyframes |

---

## 🔍 Deep-Dive: Legacy Codebase Analysis (Previous)

The previous architectural model was a single-view, static customizer built on top of declarative native elements.

### 1. Technology Choices & Libraries
*   **Google `<model-viewer>` & `<model-viewer-effects>`**: Utilized declarative HTML tags to embed and display a single `.glb` model. It integrated simple post-processing bloom layers using inner declarative `<effect-composer>` tags.
*   **ES-Module-Shims & Import Maps**: Relied on CDN-fetched polyfills (`https://ga.jspm.io/npm:es-module-shims@1.7.1/...`) to handle ES modules across aging web browsers.
*   **FontAwesome CDN**: Fetched raw vector icons over cdnjs to decorate control icons.
*   **Vanilla CSS Variables**: Applied CSS `:root` variables to toggle accent colors and loading states manually using hardcoded selectors.

### 2. Rendering Flow & Event Loop
*   **Material Bindings**: Depended on remote asset downloads containing rigid materials JSON (`materials-2.json`). It manipulated standard meshes inside `<model-viewer>` using programmatic attribute adjustments.
*   **Hash-Based Routing**: Listened on `window.location.hash` changes to show or hide panels (`#builder`) using a basic `display: flex/none` utility.
*   **Interactive Controls**: Set up global, imperative JavaScript loops to bind event listeners (`addEventListener`). It programmatically generated and injected raw `HTMLDivElement` tags (swatches circles) into the DOM.
*   **Snapshot Logic**: Manually mutated `<model-viewer>`'s `.cameraOrbit` across 4 preset coordinates inside `captureViews()`. It used `.toBlob()` to generate local blobs, outputting them in a DOM carousel list that relied on hardcoded touch coordinates on mobile screens.

---

## 🚀 Deep-Dive: Modern Architecture Analysis (Current)

The current iteration completely rewites the legacy architecture, replacing general web abstractions with a high-performance React component hierarchy backed by strict type safety.

### 1. The React & TypeScript Orchestration
*   **Modular Architecture**: Isolated concerns into clear, independent pieces:
    *   `App.tsx` (the root view controller, dashboard organizer, and environment compiler).
    *   `ThreeCanvas.tsx` (the dedicated WebGL scene lifecycle context).
    *   `Sidebar.tsx` (the custom, responsive user selector with sliding action steps).
    *   `materialsConfig.ts` & `modelsList.ts` (strongly-typed structural metadata).
*   **State Integrity & Constraints**: Governed by the `CustomizerState` interface. It synchronizes option states (Neon tube lighting, acrylic color matching, active LED color overrides) in real-time between the Sidebar form selectors and the WebGL scene.
*   **Lazy Loading**: Defers loading the massive WebGL rendering engine via React `lazy` and `Suspense` until the user chooses a model, optimization of initial page load budget.

### 2. High-Performance Three.js Graphics Pipeline
*   **Custom Environments**: Loads realistic urban HDR scenes via `RGBELoader` (`studio_small_09_1k.hdr`) with equirectangular spherical reflection mapping, giving chrome/brass edge casings vibrant, reflective details.
*   **PBR Materials & Puncture Shading**: Loads and repeats custom PBR texture maps (diffuse, normal, physical roughness, metalness maps) for custom finishes:
    *   *Mirror* (highly reflective metallics).
    *   *Powder-coated* (speckled matte roughness).
    *   *Brushed* (unidirectional orbital light streaks).
    *   *Punch* (procedurally masked perforated sheets using physical transparency maps with custom mask `alphaCutoff`).
*   **Atmospheric Floor Reflection & Lighting**: Draws a physical wet-like asphalt floor plane with strict shadow-casting parameters and real-time physical light entities. This dynamically reflects glowing sign casing backlights and neon tube emissions back into the camera viewport.
*   **Dynamic Geometric Fallbacks**: If a GLB model is missing or fails to load, `ThreeCanvas` automatically builds smooth 3D extruded shapes (rounded rectangles, diamonds, shields, ellipses, etc.) based on custom bezier spline paths, ensuring a seamless user experience.
*   **Synchronized Snapshot Vector Captures**: When `three:capture-all-angles` is detected, the canvas temporarily disables the active background environment and scales lighting. It programmatically rotates through distinct polar coordinates to trigger 4 automatic high-contrast transparent PNG downloads.

---

## 💎 Why This Evolution Matters: A Value Statement

1.  **Frictionless Customization**: Moving from 1 hardcoded model to **8 distinct curated casing presets** allows businesses to preview an entire line of architectural signs instantly.
2.  **Visual Authenticity**: Shifting from `<model-viewer>`'s basic rendering to **raw PBR shaders, HDR reflections, and ambient wet pavement floors** renders high-fidelity simulations that accurately match real-world physical sign designs.
3.  **Enterprise-Ready Reliability**: The introduction of **procedural 3D geometry fallbacks** means server connection issues or missing files will never freeze the user's interface.
