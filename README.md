# LSC Intelligent Content — Multi-Page Single Zip Template

![Immunexis Multipage SingleZip Presentation](assets/Immunexis_Multipage_SingleZip_Presentation.gif)

## Overview

This repo demonstrates the **simplest possible** multi-page zip and navigation for Life Sciences Cloud (LSC) Intelligent Content (CLM). It is a 5-slide eDetailer presentation packaged as a single zip file that runs inside the LSC CLM player on iPad.

**Intentionally minimal.** All JavaScript is inline in each HTML file — we deliberately do not separate it into external `.js` files. The goal is to show how easy it is to build navigable multi-slide content with just HTML and the `PresentationPlayer` native bridge API.

### One Zip per Slide vs. Single Zip

The **recommended approach** for production Intelligent Content is to create **one zip per slide** (one Product Guidance per zip). This gives you independent version control, granular tracking, and the ability to reorder or swap slides without repackaging the entire presentation.

This repo is for teams that want **all slides bundled into a single zip** — useful for prototyping, demos, or presentations where slides are tightly coupled and always delivered together.

### Quick Start

A ready-to-upload zip is included in the repo: **`output/Immunexis_Multipage_SingleZip.zip`**. Upload it directly to Admin Console > Intelligent Content to see it in action.

### Documentation

- [Presentation Player Functions](https://help.salesforce.com/s/articleView?id=ind.lsc_presentation_player_functions.htm&type=5) — `PresentationPlayer.gotoSlide()`, `goNextPage()`, `goPreviousPage()`, and other native bridge methods
- [Presentation Zip File Sources](https://help.salesforce.com/s/articleView?id=ind.lsc_presentation_zip_file_sources.htm&type=5) — How to structure and package zip files for Intelligent Content

## Folder Structure

```
output/
├── 01_index.html          ← Slide 1 (loaded first by CLM player)
├── 01_thumbnail.jpg       ← Thumbnail for slide 1
├── 02_index.html          ← Slide 2
├── 02_thumbnail.jpg
├── 03_index.html          ← Slide 3
├── 03_thumbnail.jpg
├── 04_index.html          ← Slide 4
├── 04_thumbnail.jpg
├── 05_index.html          ← Slide 5
├── 05_thumbnail.jpg
├── assets/                ← Slide background images
│   ├── 01_Intro.png
│   ├── 02_Efficacy.png
│   ├── 03_Safety.png
│   ├── 04_Dosing.png
│   └── 05_PatientSupport.png
├── prompts.md             ← AI image generation prompts
└── README.md              ← This file
```

### Naming Convention

- **`XX_index.html`** — The numeric prefix (`01_`, `02_`, etc.) determines the page order in the CLM player. The filename must end in `_index.html`.
- **`XX_thumbnail.jpg`** — Thumbnail images shown in the presentation page list. Must match the numeric prefix of the corresponding HTML file.
- **`assets/`** — Contains slide background images referenced by the HTML files.

## How to Package as a Zip

1. Select **all files and folders inside** the `output/` directory (not the `output/` folder itself).
2. Compress into a single `.zip` file.

On macOS:
```bash
cd output
zip -r ../Immunexis_Multipage_SingleZip.zip . -x "prompts.md" -x "README.md"
```

The resulting zip should have this structure at its root:
```
Immunexis_Multipage_SingleZip.zip
├── 01_index.html
├── 01_thumbnail.jpg
├── 02_index.html
├── 02_thumbnail.jpg
├── 03_index.html
├── 03_thumbnail.jpg
├── 04_index.html
├── 04_thumbnail.jpg
├── 05_index.html
├── 05_thumbnail.jpg
└── assets/
    ├── 01_Intro.png
    ├── 02_Efficacy.png
    ├── 03_Safety.png
    ├── 04_Dosing.png
    └── 05_PatientSupport.png
```

**Do not** include `prompts.md` or `README.md` in the zip — they are for reference only.

## How to Upload to LSC via Admin Console

### Step 1: Navigate to Intelligent Content

1. Log in to your Salesforce org.
2. Open the **App Launcher** (waffle icon, top left) and search for **Admin Console**.
3. In the Admin Console, click **Intelligent Content** in the left sidebar.

### Step 2: Upload the Zip

1. Click the **Upload** button (or **New Presentation** depending on your version).
2. Select `Immunexis_Multipage_SingleZip.zip` from your local machine.
3. LSC will process the zip and automatically detect the 5 HTML pages based on the `XX_index.html` naming convention.
4. A **Presentation** record is created, along with 5 **PresentationPage** records (one per slide).
5. The thumbnail images (`XX_thumbnail.jpg`) are automatically associated with their corresponding pages.

### Step 3: Configure the Presentation

1. Set the **Presentation Name** (e.g., "Immunexis eDetailer").
2. Set the **Status** to **Active** (or set Activation/Deactivation dates for scheduled availability).
3. Optionally configure:
   - **Player Gesture** — swipe behavior between slides
   - **Enable Double Tap Zoom** — allow zoom on content
   - **Enable Pinch Zoom** — allow pinch-to-zoom
   - **Send by Email** — allow reps to email the presentation

### Step 4: Assign Topics and Products

1. On the Presentation record, assign **Topics** to categorize the content (e.g., "Immunology").
2. Assign **Products** to individual pages or to all pages to link the presentation to your product catalog.

### Step 5: Distribute to Territories

1. On the Presentation record, click **Distribute** (or use the territory distribution section).
2. Select the territories whose field reps should receive this content.
3. Choose whether to **include child territories** (cascades to all sub-territories) or target specific territories only.

### Step 6: Sync to iPad

1. Go to **Admin Console > Mobile Settings** and regenerate the **metadata cache** so the new presentation is included in the next sync.
2. On the iPad, open the LSC Mobile app and perform a **sync** to download the presentation.
3. The presentation will appear in the rep's content library, ready for use during calls.

### Verifying the Upload

After upload, you can verify the content was processed correctly:

- **Presentation record** — should show Status, page count, and content details
- **PresentationPage records** — should list 5 pages in order (01 through 05) with thumbnails
- **Admin Console > Intelligent Content** — the presentation should appear in the content list with a "5 pages" indicator

## How the Navigation Bar Works

Each HTML slide contains a `<nav>` bar at the top with links to all 5 slides. Navigation uses the **PresentationPlayer** native bridge API, which is injected into the JavaScript runtime by the LSC CLM player on iPad.

### The PresentationPlayer.gotoSlide() Method

```javascript
PresentationPlayer.gotoSlide(PageId, slideName, animation)
```

| Parameter   | Type           | Description |
|-------------|----------------|-------------|
| `PageId`    | String or null | Salesforce ID of the target PresentationPage record. Pass `null` to navigate by slide name. |
| `slideName` | String         | The HTML filename within the zip (e.g., `"02_index.html"`). Used when PageId is null. |
| `animation` | String or null | Transition animation. Pass `null` for default. |

### How It Is Used in This Template

Each nav link calls `gotoSlide` with `null` for PageId and the target HTML filename:

```html
<nav>
  <a onclick="PresentationPlayer.gotoSlide(null,'01_index.html',null)">Overview</a>
  <a onclick="PresentationPlayer.gotoSlide(null,'02_index.html',null)">Efficacy</a>
  <a onclick="PresentationPlayer.gotoSlide(null,'03_index.html',null)">Safety</a>
  <a onclick="PresentationPlayer.gotoSlide(null,'04_index.html',null)">Dosing</a>
  <a onclick="PresentationPlayer.gotoSlide(null,'05_index.html',null)">Resources</a>
</nav>
```

The current slide's link has `class="active"` for visual highlighting.

### Important: Always Use 3 Arguments

```javascript
// CORRECT — 3 arguments
PresentationPlayer.gotoSlide(null, "02_index.html", null);

// WRONG — 1 argument (CLM player treats it as PageId and appends .pdf)
PresentationPlayer.gotoSlide("02_index.html");
```

### Other Navigation Methods

| Method | Description |
|--------|-------------|
| `PresentationPlayer.goNextPage()` | Navigate to the next page in sequence |
| `PresentationPlayer.goPreviousPage()` | Navigate to the previous page in sequence |

### Testing Locally

The `PresentationPlayer` object is only available inside the LSC CLM player on iPad. When previewing HTML files in a desktop browser, navigation clicks will produce a JavaScript error (`PresentationPlayer is not defined`). This is expected — the content is designed to run within the CLM runtime.

**Do not** define a `PresentationPlayer` shim in your JavaScript. The native bridge object is injected by the CLM runtime and overwriting it will cause "Content could not load" errors on the device.

## Slide Layout

Each HTML file follows this structure:

- **Nav bar**: 44px tall, HTML with inline styles and `onclick` handlers
- **Content area**: 1024 x 724px, displays a single background image from the `assets/` folder
- **Total viewport**: 1024 x 768px (iPad logical resolution, rendered at 2x Retina = 2048 x 1536 physical pixels)

The background images are generated at **2048 x 1448 pixels** (2x the 1024x724 content area) for Retina sharpness on iPad Pro 13".

## Customizing This Template

### Changing Slide Content

1. Generate new background images at 2048 x 1448 pixels using the prompts in `prompts.md` as a starting point.
2. Replace the corresponding PNG in the `assets/` folder.
3. Regenerate thumbnails at 400 x 300 pixels.

### Adding or Removing Slides

1. Add/remove `XX_index.html` and `XX_thumbnail.jpg` files, keeping the numeric prefix sequence.
2. Update the `<nav>` links in **every** HTML file to reflect the new slide list.

### Changing the Color Scheme

The nav bar uses an inline CSS gradient. Update this line in each HTML file:

```css
nav { background: linear-gradient(135deg, #2d1854, #4a1a7a); }
```
