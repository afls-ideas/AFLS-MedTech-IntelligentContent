# MedTech Demo — LSC Intelligent Content eDetailer Presentations

This repo contains five eDetailer presentation suites for a MedTech device portfolio, built for **Salesforce Life Sciences Cloud (LSC) Intelligent Content (CLM)**. Each presentation is structured as **one zip per slide** (the recommended production pattern), enabling independent version control, granular rep tracking, and flexible content sequencing.

---

## Product Portfolio

### 1. Robotic Surgery — VisuMax
A next-generation robotic surgical platform delivering sub-millimeter precision, tremor filtration, and 3D HD visualization.

| # | Slide | LSC Record Name |
|---|-------|----------------|
| 1 | Overview | `VisuMax Overview` |
| 2 | Technology | `VisuMax Technology` |
| 3 | Clinical Outcomes | `VisuMax Clinical` |
| 4 | System Portfolio | `VisuMax Portfolio` |
| 5 | Why VisuMax | `VisuMax WhyVisuMax` |

**Output folder:** `output_robotic_surgery/`
**Nav color:** Deep navy → midnight blue (`#0a1628 → #0d2d5e`)

---

### 2. Orthopedic Implants — TitanHip & TitanKnee + OrthoNav
Primary and revision hip and knee reconstruction systems with OrthoNav robotic arm guidance for sub-degree implant alignment accuracy.

| # | Slide | LSC Record Name |
|---|-------|----------------|
| 1 | Overview | `TitanHip Overview` |
| 2 | OrthoNav Guidance | `TitanHip Technology` |
| 3 | Clinical Outcomes | `TitanHip Clinical` |
| 4 | Implant Portfolio | `TitanHip Portfolio` |
| 5 | Why TitanHip & Knee | `TitanHip WhyTitanHip` |

**Output folder:** `output_orthopedic/`
**Nav color:** Deep charcoal → warm charcoal (`#1a1410 → #3d2b1a`)

---

### 3. Spine & Neuro — SpinePath + NeuroNav
Comprehensive spine reconstruction covering interbody fusion (ACDF, TLIF, LLIF), pedicle screw stabilization, and the NeuroNav real-time surgical navigation system.

| # | Slide | LSC Record Name |
|---|-------|----------------|
| 1 | Overview | `SpinePath Overview` |
| 2 | NeuroNav Navigation | `SpinePath Technology` |
| 3 | Clinical Outcomes | `SpinePath Clinical` |
| 4 | Implant Portfolio | `SpinePath Portfolio` |
| 5 | Why SpinePath | `SpinePath WhySpinePath` |

**Output folder:** `output_spine_neuro/`
**Nav color:** Deep forest green → teal (`#0d2b1a → #0d3b2e`)

---

### 4. Cardiac & Vascular — HeartGate TAVR + VascuStent
Transcatheter aortic valve replacement with a repositionable self-expanding design, plus a comprehensive peripheral and coronary stent and balloon portfolio.

| # | Slide | LSC Record Name |
|---|-------|----------------|
| 1 | Overview | `HeartGate Overview` |
| 2 | TAVR Technology | `HeartGate Technology` |
| 3 | Clinical Outcomes | `HeartGate Clinical` |
| 4 | Cardiac & Vascular Portfolio | `HeartGate Portfolio` |
| 5 | Why HeartGate | `HeartGate WhyHeartGate` |

**Output folder:** `output_cardiac_vascular/`
**Nav color:** Deep crimson → dark red (`#2b0a0a → #5c1010`)

---

### 5. Endoscopy — ClearView 4K + ScopeGuide
4K Ultra HD endoscopy tower with AI-assisted lesion detection, flexible endoscopes for gastroscopy, colonoscopy, and bronchoscopy, and ScopeGuide real-time 3D scope position mapping.

| # | Slide | LSC Record Name |
|---|-------|----------------|
| 1 | Overview | `ClearView Overview` |
| 2 | 4K Imaging Technology | `ClearView Technology` |
| 3 | ScopeGuide Navigation | `ClearView Clinical` |
| 4 | Endoscopy Portfolio | `ClearView Portfolio` |
| 5 | Why ClearView | `ClearView WhyClearView` |

**Output folder:** `output_endoscopy/`
**Nav color:** Deep cerulean → dark blue (`#0a1a2b → #0d2d4a`)

---

## Repository Structure

```
output_robotic_surgery/
├── 01_VisuMax_Overview/
│   ├── 01_index.html        ← Slide HTML (entry point for this zip)
│   ├── 01_thumbnail.jpg     ← 400×300px thumbnail
│   └── assets/
│       └── 01_Intro.png     ← 2048×1448px slide background image
├── 01_VisuMax_Overview.zip  ← Upload this zip to LSC
├── 02_VisuMax_Technology/
├── 02_VisuMax_Technology.zip
├── 03_VisuMax_Clinical/
├── 03_VisuMax_Clinical.zip
├── 04_VisuMax_Portfolio/
├── 04_VisuMax_Portfolio.zip
├── 05_VisuMax_WhyVisuMax/
└── 05_VisuMax_WhyVisuMax.zip

images/
├── robotic_surgery/         ← Source PNG images (2048×1448px)
├── orthopedic/
├── spine_neuro/
├── cardiac_vascular/
└── endoscopy/

prompts_robotic_surgery.md   ← AI image generation prompts
prompts_orthopedic.md
prompts_spine_neuro.md
prompts_cardiac_vascular.md
prompts_endoscopy.md
```

---

## How Navigation Works (Multi-Zip Pattern)

Each zip contains exactly one slide (`01_index.html`). The nav bar navigates between separately uploaded zips using `PresentationPlayer.gotoSlide()` with the **LSC Product Guidance record name** as the first argument:

```html
<!-- On the Overview slide (currently active) -->
<nav>
  <a class="active" onclick="PresentationPlayer.gotoSlide(null,'01_index.html',null)">Overview</a>
  <a onclick="PresentationPlayer.gotoSlide('VisuMax Technology','01_index.html',null)">Technology</a>
  <a onclick="PresentationPlayer.gotoSlide('VisuMax Clinical','01_index.html',null)">Clinical</a>
  <a onclick="PresentationPlayer.gotoSlide('VisuMax Portfolio','01_index.html',null)">Portfolio</a>
  <a onclick="PresentationPlayer.gotoSlide('VisuMax WhyVisuMax','01_index.html',null)">Why VisuMax</a>
</nav>
```

**Critical:** The first argument (`'VisuMax Technology'`) must match the **exact name of the Product Guidance record in LSC** — including capitalization and spacing. A mismatch produces a "page can't be accessed" error on device.

- The **active slide** uses `null` as the first argument (navigate within current zip).
- All other slides use the LSC record name to jump between zips.
- Always pass **3 arguments** — passing only 1 causes the CLM player to treat the string as a PageId and append `.pdf`.

---

## How to Upload to LSC

1. Go to **Admin Console > Intelligent Content**.
2. Upload each `.zip` file individually — **5 uploads per product = 25 uploads total**.
3. Set the **Product Guidance record name** to match exactly what is in the nav tables above.
4. Set **Status** to **Active**.
5. Assign **Products** and **Topics** as appropriate.
6. **Distribute** to the relevant territories.
7. Regenerate the **metadata cache** so the content syncs to field rep iPads.

---

## Generating Slide Images

Each product has a dedicated prompt file with 5 detailed Nano Banana-style image generation prompts — one per slide, at **2048×1448 pixels**.

| Product | Prompt File |
|---------|------------|
| Robotic Surgery | `prompts_robotic_surgery.md` |
| Orthopedic Implants | `prompts_orthopedic.md` |
| Spine & Neuro | `prompts_spine_neuro.md` |
| Cardiac & Vascular | `prompts_cardiac_vascular.md` |
| Endoscopy | `prompts_endoscopy.md` |

Paste prompts into **Google Gemini**, **ChatGPT (DALL-E 3)**, **Midjourney** (`--ar 256:181`), or **Adobe Firefly**. Save outputs as PNG into the corresponding `images/<product>/` folder.

Thumbnails are auto-generated from the slide images at **400×300px**:
```bash
sips -z 300 400 assets/01_Intro.png --out 01_thumbnail.jpg
```

---

## Slide Dimensions

| Element | Logical (CSS) | Physical (Retina 2×) |
|---------|--------------|----------------------|
| Full viewport | 1024 × 768px | 2048 × 1536px |
| Nav bar | 1024 × 44px | — |
| Slide content area | 1024 × 724px | 2048 × 1448px |
| Thumbnail | 400 × 300px | — |
