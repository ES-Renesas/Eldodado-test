This is the ultimate master prompt. It synthesizes your entire 3-month product strategy, the specific hardware targets (Renesas/STM32), your team's constraints, and the zero-build GitHub Pages architecture into one massive set of instructions.

Feeding this to an AI like Claude will give it the exact context it needs to act as your Senior Frontend Engineer and deliver the complete MVP in one shot.

---

### 📋 Copy/Paste This Master PRD Prompt to Claude:

**System Role:** You are an expert Principal Frontend Engineer and UX Architect. Your task is to build a production-ready, single-page application based on the following Product Requirements Document (PRD).

# PRD: Eldorado Model Zoo Explorer (V1 MVP)

## 1. Business Context & Product Strategy

We are a small, agile embedded AI team (1 Product Manager, 2 Engineers) executing a 3-month (6-sprint) roadmap. We optimize open-source neural networks (MobileNet, YOLO, etc.) by quantizing them to Int8 to run on resource-constrained microcontrollers (MCUs) like the Renesas RA8 and STM32.

Our core value proposition is **Hardware Optimization**, not training models from scratch.

**The Goal:** We need a static, highly interactive "Model Explorer" web interface hosted directly in our GitHub repository. It acts as a self-serve catalog where firmware engineers can instantly filter our quantized models by strict hardware limits (Flash/RAM) and visualize the trade-off between Latency (FPS) and Accuracy. The design inspiration is the Hailo Model Zoo Explorer.

## 2. Strict Technical Constraints (Zero-Build Architecture)

* **Hosting:** Native GitHub Pages.
* **Architecture:** 100% Client-Side. NO build steps. NO React, Vue, Node.js, or `npm`.
* **Output:** The entire application must be contained within a single `index.html` file (with embedded JS/CSS) so it can be dragged, dropped, and run locally by double-clicking.
* **Tech Stack (CDNs ONLY):**
* **Tailwind CSS (via Play CDN):** For enterprise SaaS styling. Clean, light mode, data-dense, with slate grays and crisp blues.
* **Plotly.js (via CDN):** For the interactive scatter plot.
* **Netron (via CDN/Iframe):** For client-side model visualization.
* **Vanilla JavaScript:** To handle all state, filtering, and DOM manipulation.



## 3. Core Features & UI Layout

### A. Global Header

* Clean navigation bar with the title "Eldorado Model Zoo Explorer".
* Two primary navigation tabs/toggles: **"Model Catalog"** (default) and **"Pre-Flight Check"**.

### B. View 1: The Model Catalog (Split Layout)

**Left Sidebar (Interactive Filters):**
Changing these filters must instantly update both the Graph and the Table using Vanilla JS array filtering.

* Dropdown: "Task" (All, Classification, Object Detection, Keyword Spotting)
* Dropdown: "Family" (All, MobileNet, TinyML, Custom, EfficientNet)
* Range Slider: "Max Peak RAM (KB)" [0 to 1000]
* Range Slider: "Max Flash (KB)" [0 to 5000]
* Range Slider: "Min FPS" [0 to 150]
* Checkbox: "Show Int8 Quantized Only"

**Main Content (Toggle between Graph & Table):**

* **The Graph (Plotly.js):** * X-Axis: "Latency (FPS)". **CRITICAL: Must be a logarithmic scale.**
* Y-Axis: "Accuracy (%)".
* Bubbles color-coded by "Family". Hovering displays a tooltip with: Model Name, Task, FPS, Accuracy, RAM, and Flash.


* **The Table:** * Dense grid with columns: Model Name, Family, Task, Flash (KB), Peak RAM (KB), MACs (M), FPS, Accuracy (%).
* Headers must be clickable to sort (ascending/descending). Highlight the RAM and FPS columns.



### C. View 2: The "Pre-Flight Check" (Client-Side Value Add)

This section proves our toolchain works without sending proprietary data to a server.

* **Drag & Drop Zone:** "Drop your .tflite model here to inspect."
* **Visualizer:** Embed the Netron visualizer (`https://netron.app/`) using their CDN script to render the uploaded model graph directly in the browser.
* **C-Header Generator:** Provide a dummy button labeled "Generate C-Header". When clicked, show a `<pre><code>` block with a template `model_metadata.h` containing placeholders for `TENSOR_ARENA_SIZE_BYTES`, `MODEL_INPUT_WIDTH`, `PREPROCESS_ZERO_POINT`, and `PREPROCESS_SCALE_FACTOR`.

## 4. The Initial Dataset

Hardcode this JSON array into a JavaScript variable to act as the database:

```json
[
  { "id": 1, "name": "MobileNetV2 (0.1x)", "task": "Classification", "flash": 350, "ram": 85, "macs": 5.4, "fps": 125, "accuracy": 45.0, "quantized": true, "family": "MobileNet" },
  { "id": 2, "name": "MicroNet-M2", "task": "Classification", "flash": 400, "ram": 90, "macs": 12.0, "fps": 55, "accuracy": 52.0, "quantized": true, "family": "TinyML" },
  { "id": 3, "name": "MobileNetV1 (0.25x)", "task": "Classification", "flash": 450, "ram": 110, "macs": 14.0, "fps": 45, "accuracy": 41.5, "quantized": true, "family": "MobileNet" },
  { "id": 4, "name": "MCUNet (TinyNAS)", "task": "Classification", "flash": 480, "ram": 120, "macs": 25.0, "fps": 28, "accuracy": 62.0, "quantized": true, "family": "TinyML" },
  { "id": 5, "name": "MobileNetV2 (0.35x)", "task": "Classification", "flash": 650, "ram": 180, "macs": 30.0, "fps": 22, "accuracy": 59.8, "quantized": true, "family": "MobileNet" },
  { "id": 6, "name": "GhostNet (0.5x)", "task": "Classification", "flash": 1300, "ram": 200, "macs": 40.0, "fps": 16, "accuracy": 66.2, "quantized": true, "family": "Custom" },
  { "id": 7, "name": "MobileNetV3-Small", "task": "Classification", "flash": 1500, "ram": 280, "macs": 56.0, "fps": 12, "accuracy": 66.5, "quantized": true, "family": "MobileNet" },
  { "id": 8, "name": "SqueezeNet v1.1", "task": "Classification", "flash": 1200, "ram": 400, "macs": 350.0, "fps": 2, "accuracy": 58.0, "quantized": true, "family": "Custom" },
  { "id": 9, "name": "MobileNetV2 (1.0x)", "task": "Classification", "flash": 3400, "ram": 600, "macs": 300.0, "fps": 2, "accuracy": 71.0, "quantized": true, "family": "MobileNet" },
  { "id": 10, "name": "EfficientNet-Lite0", "task": "Classification", "flash": 4500, "ram": 900, "macs": 390.0, "fps": 1, "accuracy": 74.0, "quantized": true, "family": "EfficientNet" }
]

```

## 5. Execution Instructions

Write the complete, robust `index.html` file containing the HTML structure, the Tailwind utility classes, and the Vanilla JS required to make the filters, chart, and toggles function perfectly. Ensure the code is heavily commented so my engineering team can easily swap out the dummy data for our real `.json` file later.

---

Would you like me to outline the specific `Dockerfile` that your 2 engineers should build in Sprint 1 to ensure their quantization environment is locked down and perfectly reproducible?