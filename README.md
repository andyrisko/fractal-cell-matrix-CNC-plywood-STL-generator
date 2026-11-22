# Cell Studio — Fractal Matrix & Plywood STL Generator

A self-contained, browser-based generative design tool that creates organic fractal cellular patterns and converts them into **watertight 3D STL relief models** ready for **CNC routing** or **3D printing**, complete with a real-time **WebGL 3D plywood preview**.

---

## ✨ Key Features

- **Progressive 1024 × 1024 Rendering:** Calculates fast low-resolution previews (`128×128` → `256×256` → `512×512`) while adjusting parameters and refines to full `1024×1024` resolution without freezing the browser.
- **Domain-Warped Voronoi Engine:** Distorts Euclidean cellular regions with multi-octave Fractal Brownian Motion (`fBm`) noise to produce organic, natural-looking cell walls.
- **Customizable Cell Falloff & Shading:** Choose from 7 mathematical elevation profiles (*Smoothstep, Linear, Cosine, Inverse Cosine, Quadratic Dome, Inverse Quadratic Spike, Half-Sine*), invert relief direction, and adjust core plateau size.
- **Real-Time WebGL 3D Plywood Simulation:** Preview how the carved relief will look when milled into layered wood veneers (*Baltic Birch, Walnut & Maple, Dyed Spectra Ply,* or *Solid Matte*), complete with cross-laminated grain and dark resin glue lines.
- **Watertight Binary STL & PNG Export:** Generates closed-manifold binary `.stl` files with configurable millimeter dimensions, Gaussian smoothing, and raised/recessed frames, plus lossless `1024×1024` 2D heightmap `.png` exports.

---

## 🚀 Quick Start

1. **Open the App:** Double-click the HTML file (`Cell-Studio.html`) to open it in any modern web browser (Chrome, Edge, Firefox, Safari). No installation, server, or internet connection is required.
2. **Explore Shapes:** Click **New Seed** or adjust the **Fractal & Cell Sliders** in the left sidebar.
3. **Inspect in 3D & 2D:**
   - **Left-click + Drag:** Rotate 3D model
   - **Right-click + Drag** (or **Shift + Drag**): Pan camera
   - **Scroll Wheel:** Zoom in/out
   - Switch between **3D Plywood Preview** and **2D Matrix** using the top tabs.
4. **Export:** Configure physical dimensions in millimeters and click **Export Binary STL (3D)** for CNC/3D printing or **Export PNG** for a 2D heightmap.

---

## 🎛️ Controls Reference

### 1. Pattern & Fractal Geometry
| Control | Description |
| :--- | :--- |
| **Random Seed / New Seed** | Controls the pseudo-random starting layout. Using the same seed and settings always reproduces the exact same pattern. |
| **Initial Cell Count** | Sets the number of seed points (`4`–`180`) used to partition the canvas. |
| **Fractal Octaves** | Number of layered noise frequencies (`1`–`8`). Higher values add finer organic detail along cell boundaries. |
| **Fractal Roughness** | Controls how strongly each finer octave influences the shape (`0.15`–`0.85`). |
| **Noise Base Scale (px)** | Size of the primary distortion waves (`50`–`700 px`). |
| **Fractal Warp (px)** | Strength of the organic distortion applied to cell walls (`0`–`150 px`). |
| **Wall Width (px)** | Thickness of the dividing membranes between cells (`1`–`14 px`). |
| **Core Area (%)** | Expands the flat central plateau inside each cell (`0%`–`90%`). |

### 2. Shading & Falloff
| Control | Description |
| :--- | :--- |
| **Gradient Dir** | Switches between **Dark core → Light wall** (recessed cells, raised walls) and **Light core → Dark wall** (domed/peaked cells, recessed walls). |
| **Falloff Curve** | Mathematical profile controlling the slope from cell center to wall (*Smoothstep, Linear, Cosine, Inv Cosine, Quadratic, Inv Quadratic, Half-Sine*). |
| **Cell Walls / Gradient / Core Centers / Dotted Walls** | Toggles wall visibility, interior shading gradients, 2D core center markers, and dashed wall patterns. |

### 3. 3D Plywood Preview
| Control | Description |
| :--- | :--- |
| **Plywood Style** | Visual material preset for the 3D WebGL preview (*Baltic Birch, Walnut & Maple, Dyed Spectra Ply, Solid Matte*). |
| **Ply Layers** | Number of simulated wood veneer layers (`3`–`25`) sliced through the total relief height. |

### 4. 3D STL Export & Frame
| Control | Description |
| :--- | :--- |
| **Width (mm)** | Physical X/Y width and height of the square exported model (`10`–`500 mm`). |
| **Base (mm)** | Thickness of the solid backing plate underneath the lowest carved point (`0.2`–`50 mm`). |
| **Relief (mm)** | Vertical carving depth from the base top to the highest peak (`0.1`–`100 mm`). |
| **Frame Type & Rim Width** | Adds a **Raised Rim**, **Recessed Rim**, or **No Frame** around the perimeter with a smooth beveled transition. |
| **Smooth** | Applies `0`–`10` Gaussian blur passes to soften sharp pixel transitions for cleaner CNC toolpaths. |
| **STL Mesh** | Output grid resolution (`256×256`, `512×512`, or `1024×1024`). Higher resolutions capture finer detail at larger file sizes. |

---

## 🛠️ How It Works Under the Hood

1. **Domain-Warped Voronoi Partitioning:** Each pixel's `(x, y)` coordinates are displaced using two offset `fBm` noise fields before measuring squared distance to jittered seed sites.
2. **Connected-Component Labeling:** A flood-fill pass separates cells that split into multiple islands under heavy warping and computes each region's centroid.
3. **Two-Pass Chamfer Distance Transform:** Measures exact pixel distance from every interior point to its nearest cell membrane.
4. **Pole of Inaccessibility & Radial Normalization:** Finds a guaranteed interior core point for concave cells and computes a smooth `0.0 → 1.0` radial coordinate between the core and wall.
5. **Manifold Mesh Generation:** Downsamples, smooths, and frames the heightmap into millimeters, then writes a binary little-endian `.stl` buffer containing the top relief surface, flat `Z = 0` bottom plate, and four stitched perimeter walls.

---

## 💻 Browser Requirements

- Any modern desktop or mobile browser with **HTML5 Canvas** and **WebGL 1.0** support (Chrome, Firefox, Edge, Safari).
- Supports `OES_element_index_uint` for high-density 3D preview meshes.
