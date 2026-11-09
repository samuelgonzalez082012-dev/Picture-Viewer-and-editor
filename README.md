# Bare Bones Photo Editor — V1.9 development build

A dependency-light, browser-based raster photo editor. Open `index.html` directly in a modern Chromium-based browser, Firefox, or Safari. No build step or server is required for the core editor.

## Included in this build

### Existing tools preserved
- A home screen lists projects saved in this app; select one to continue editing, or open an image or `.bbpe` project file.
- Open images through the file picker, drag and drop, or clipboard paste.
- Import PNG, JPG/JPEG, WebP and BMP images.
- Local AI portrait-background removal with transparent output or locally generated studio, sunset, ocean, forest, aurora and solid-color backgrounds.
- Editable AI cutout masks: restore subject or erase background with a brush, undo brush strokes, then apply the refined mask.
- Local brush-based content-aware erase that fills from nearby pixels; this is not generative AI and works best on small objects/simple surroundings.
- Brightness, contrast, saturation, warmth, hue, sepia, grayscale, inversion.
- Exposure, highlights, shadows, whites, blacks and vignette.
- Presets, custom presets, undo/redo and clickable history.
- Crop with freeform and common aspect ratios, rotate 90 degrees, flip horizontally/vertically.
- Hold-to-compare, zoom, fit, space-drag pan, export to PNG/JPEG/WebP.
- Save/open `.bbpe` project files.
- Per-document undo/redo history and live adjustment/filter previews.

### V1.1–V1.9 development features
- Brush, pencil and eraser drawing with color, eyedropper/Alt-click, size, softness, opacity, custom cursor and Shift-straight lines.
- HSL mixer for red, orange, yellow, green, aqua, blue, purple and magenta.
- Additional tonal/color controls: clarity, sharpening approximation, film grain, temperature, tint and three tonal-curve region controls.
- Composition layers: editable text with font/style/alignment, rectangles, ellipses, lines, polygons, imported image layers and freehand paint strokes.
- A left-hand icon toolbar for selecting and applying the editor's image, selection, paint, layer and transform tools.
- Rectangular and elliptical selections, freehand and polygon lasso, connected-region magic wand, invert, feather, expand/contract, deselect and in-app copy/paste.
- Layer visibility, selection, thumbnails, locking, duplication, grouping, merge-down, flattening, clipping, opacity, blend modes, reordering, rename and position/size controls.
- Filter controls for Gaussian blur, motion blur, unsharp sharpening, noise and pixelation, with live previews.
- Multiple in-memory document tabs, debounced IndexedDB autosave and startup recovery for open documents, alongside the recent-project home screen.
- Resizable/hidden controls, reorderable tool rail, system/light/dark themes, fullscreen, grid, guides, snap, rulers and keyboard shortcuts.
- Layer, filter and HSL settings are included in undo/redo and `.bbpe` project save/open.
- Arbitrary-angle straighten/rotate and image resize.
- Expanded crop aspect ratios and mouse-wheel zoom (Ctrl/Cmd + wheel).
- Keyboard shortcuts: Ctrl/Cmd+Z, Shift+Ctrl/Cmd+Z, Ctrl/Cmd+Y, Ctrl/Cmd+O, Ctrl/Cmd+S, +/- zoom, Space+drag pan.

## Run

1. Extract the ZIP.
2. Run `npm install` to install the local AI runtime, then open `index.html` in the Electron app (`npm start`).
3. From the home screen, open an image or start with the included sample image.
4. Use Export to save a flattened PNG, JPEG or WebP. Save Project creates a `.bbpe` file that preserves the base image, adjustments, HSL settings and supported layers.

## Important implementation notes / limitations

Projects saved with Save Project appear on the home screen and are stored locally in the browser's IndexedDB for the current app profile. A portable `.bbpe` file is downloaded as well.

Background removal uses the Apache-2.0 MODNet model through Transformers.js. Install dependencies with `npm install`; the first removal downloads and caches about 25 MB of model weights. Inference runs on the device and does not upload the photo. MODNet is designed for portraits, so results on other subjects can be unreliable. The generated backgrounds are local gradients/patterns, not text-prompt AI images. Use PNG export to preserve a transparent cutout.

The AI background tool first presents the proposed cutout on a checkerboard. Use Restore subject or Erase background to refine the mask, and Undo brush to remove the last stroke before applying it. Content-aware erase lets you paint an unwanted area and fills it from nearby pixels locally. It is not generative inpainting; large/complex selections may smear or leave visible seams.

This is a **development build for hands-on testing**, not a claim of Photoshop/Affinity parity. The following advanced items are not implemented as production-grade tools in this build:

- RAW camera decoding, ICC color management, TIFF/PSD interoperability and metadata round-tripping.
- Full parametric curve editor with draggable control points and per-channel RGB curves; the current curve controls are tonal-region controls.
- Professional denoise, dehaze, texture, lens profiles and chromatic-aberration correction. The unsharp filter and clarity controls are lightweight local implementations.
- Nested groups/group transforms, editable vector paths, non-destructive layer masks and professional clipping/adjustment-layer workflows. Merge-down is limited to normal-blend layers; flattening bakes the visible composite.
- Magic-wand selection uses a reduced working copy for large images; feather/expand/contract are canvas approximations. Lasso selections are not sub-pixel/vector editable.
- True dock-and-undock panels, calibrated measurement rulers, histogram overlays, batch export and external/system clipboard interoperability.

These limitations are called out rather than represented by nonfunctional buttons. Selection copy/paste is available within the current editor session; document tabs are held in memory and autosaved locally in IndexedDB.

## Bug-testing checklist

- [ ] Open a JPEG, PNG, WebP and BMP; confirm the displayed dimensions match the source.
- [ ] Drag an image onto the stage and paste an image from the clipboard.
- [ ] Exercise each adjustment and reset it by double-clicking its label.
- [ ] Exercise each HSL channel and reset HSL.
- [ ] Add text, shapes, an image layer and paint strokes; test visibility, opacity, blend mode, reorder, move, resize and delete.
- [ ] Test brush, pencil and eraser settings, eyedropper, straight lines and the custom brush cursor.
- [ ] Test layer lock, duplicate, rename, grouping, clipping, merge-down and flatten.
- [ ] Use rectangle/ellipse/lasso/polygon/magic-wand selection; test invert, feather, expand/contract, copy/paste and undo/redo.
- [ ] Test blur, motion blur, sharpen, noise and pixelate at low/high amounts and export the result.
- [ ] Open multiple documents; switch tabs, close a tab, wait for autosave, then reload and test crash recovery.
- [ ] Reorder tools and test themes, fullscreen, grid, rulers, guides, snapping and panel resizing.
- [ ] Undo and redo each of those actions; jump to earlier history entries.
- [ ] Crop at freeform and fixed ratios; rotate, flip, straighten and resize.
- [ ] Save PNG/JPEG/WebP at different quality settings.
- [ ] Save a `.bbpe` project, reopen it, and verify image, adjustments, HSL and layers.
- [ ] Remove a portrait background, undo/redo it, generate each background style, and export a transparent PNG.
- [ ] Refine a proposed background mask with restore/erase strokes; undo a brush stroke before applying.
- [ ] Paint over a small unwanted object with Content-aware erase; undo and redo the fill.
- [ ] Test keyboard shortcuts, zoom, pan and compare.
- [ ] Try a large image and note memory use, responsiveness and any browser canvas limits.

## Compatibility

The app uses standard HTML Canvas APIs and modern JavaScript. Very large images can exceed browser canvas or memory limits; preserve a backup of originals. The editor does not upload images to a server.

### Large-image rendering

Full-quality adjustment rendering is tiled in horizontal strips rather than duplicating the entire photograph into multiple full-size pixel buffers. While slider controls are being dragged, the preview is rendered at a maximum dimension of 1,200 pixels; releasing a control triggers a full-resolution render. Browser canvas limits still apply.

The editor keeps a separate source canvas and applies adjustment rendering to the display/export canvas. High-quality rendering is processed in bounded horizontal tiles; interactive slider previews use a downscaled working image. Transform and crop operations also update the normalized placement of composition layers and paint strokes.
