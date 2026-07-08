# essenceofinsrana.github.io
Website of book Essence of INS Rana
# WEBSITE FEATURES
>Best viewed on text editors at font size 12px and window width >1100px to avoid overflow of tables/flowcharts
## STRUCTURE
```
root/
│
│  [Main pages]
├── index.html
├── about.html
├── faq.html
├── contact.html
│
│  [Legal pages]
├── privacy.html
├── terms.html
├── hyperlinking.html
├── disclaimer.html
├── accessibility.html
├── sitemap.html
│
│  [Utility pages]
├── thankyou.html
├── index2.html
│
├── includes/
│   ├── config.html
│   ├── header.html
│   ├── navigation.html
│   └── footer.html
│
├── carousel-images/
│   ├── 01-Front-Cover.webp
│   ├── 02-Half-Title-Page.webp
│   ├── 03-Title-Page.webp
│   ├── 04-Table-of-Contents.webp
│   ├── 05-Essence-of-INS-Rana-Page.webp
│   └── 06-Back-Cover.webp
│
│  [Utility images]
├── favicon.png
├── 07-INS-Rana-Silhouette.webp
│
│  [Technical files]
├── style.css
├── loadcss.js
├── robots.txt
├── sitemap_map1.xml
│
│  [Information files]
├── llms.txt
├── llms-full.txt
└── README.md
```
## UX FEATURES
### Webpages
 - High score in WCAG 2.2 AA compliance 
 - Skip to Main button
 - Back to Top SVG button
 - Keyboard accessible
 - Hover effects on clickable elements
 - Main Navigation and content reorient when screen width <600px
 - Silhouette image resizes based on screen width
 - Use of .webp format images
 - Detailed FAQ page with expandable questions revealing the hidden answers
 - Contact form with captcha and honeypot security
 - High contrast levels
 - Custom browser tab colour for android phones
 - Favicon and titles for browser tabs
 - Screen reader compliant
### Carousel
 - Continuous auto-scroll (3 seconds gap)
 - Smooth transitions, dimming, and resizing effects of images
 - Pause auto-scroll when hovering over carousel image/area
 - Resume auto-scroll when hover ends on carousel area
 - Left and right navigation SVG arrows
 - Manual navigation immediately moves carousel
 - Manual navigation pauses auto-scroll for 10 seconds
 - Auto-scroll resumes automatically after 10 seconds of last manual navigation
 - Dedicated Pause/Play button below the carousel
 - Pause buttons turns to play button if paused by clicking on pause button and vice versa
 - Pause/Play button dims when carousel paused during hover over carousel area or during manual navigation
 - User-selected pause remains in effect until Play is pressed
 - Carousel image dots change colour to indicate centre image on carousel
 - Carousel image dots clickable
 - Carousel image dots if clicked/tapped result in manual navigation and pausing of auto-scroll for a period of 10 seconds
 - All controls accessible by keyboard
 - Carousel dots accessible by keyboard only when user-selected pause button pressed
 - Image Counter
 - Upon display width <600px, counter and open lightbox buttons together shift below carousel dots and pause button
 - Preload next/previous images
 - Large touch-areas for controls
### Lightbox
 - Click any carousel image to open a full-screen lightbox
 - Darkened background overlay
 - Large high-resolution image display
 - Scroll-lock of webpage while in Lightbox mode
 - Previous/Next navigation controls
 - Lightbox Close button
 - Click/tap outside image to close lightbox
 - Swipe navigation on touch displays
 - Keyboard navigation:
	 - Left Arrow → Previous image
	 - Right Arrow → Next image
	 - Esc → Close lightbox
 - Zoom levels 1x and 2x
 - Mouse Wheel zoom
 - Double-click/Double-tap zoom
 - Keyboard zoom
 - Drag-to-pan/Grab-to-pan
 - Rubber-band effect
 - Nudge-effect/boundary physics on edges while panning in 2x zoom
 - Image restricted to pan beyond lightbox boundary
 - Left and right arrows hide on first and last image
 - Nudge effect while swiping on touch displays
 - Swipe function active only in 1x zoom
 - Hide toggle of caption
 - Toggle label changes to hide or show caption and zoom
 - Image or reset zoom respectively
 - Lightbox dots update as per image
 - Clicking/tapping lightbox dots results in display of corresponding image and reset zoom
 - Lightbox dots change colour to indicate active image displayed on lightbox
 - Smooth transitions between different zoom states and image changes
 - Smooth fade/zoom animation upon lightbox open/ close
 - Upon exit lightbox, last image on lightbox become current image on carousel
 - Upon open lightbox, the current image on carousel is the current image on lightbox
 - Image counter
 - All controls accessible by touch, mouse and keyboard
 - Tap or click on carousel image opens lightbox
 - Upon display width <600px, caption and zoom toggles together shift below dots and counter
 - Carousel image auto-scroll disabled when lightbox active
 - Focus remains on Lightbox when Lightbox opens
 - Focus trap
 - Esc double-tap safety fix on mobile browsers
 - ARIA dialogues for controls
 - Preload next/previous images
 - Hover effects on controls
 - Grab and Grabbing of mouse pointer when over image in 2x zoom
 - Semi-transparent background of controls
 - Large touch-areas for controls
 - Changing image, closing lightbox, or open lightbox resets zoom
 - Pan available only when zoomed
   - Desktop: Click + drag
   - Mobile: One-finger drag
   - Keyboard: arrow buttons
 - Slight inertial dragging while user releases pan
## Technical Architecture - Carousel & Lightbox
The interactive client-side systems of this webpage are built using custom, vanilla JavaScript. Both systems utilise deterministic state machines, mathematical coordinate mappings, and simulated physical models to manage state, transitions, and performance
### Carousel Architecture & Logic
The carousel acts as a 3D-depth slider using flat transformations. It translates logical indices into visual layers while maintaining non-blocking asynchronous timers
```
  [-2] Far Prev (Opacity: 0)
              │
  [-1] Prev (Opacity: 0.55, Scale: 0.78)
              │
  [ 0] Active (Opacity: 1.00, Scale: 1.15)  <── (Current Pointer)
              │
  [+1] Next (Opacity: 0.55, Scale: 0.78)
              │
  [+2] Far Next (Opacity: 0)
```
#### State Variables
The system's behaviour is driven by seven main variables:
  - `current` (integer): Index of the active slide
  - `total` (integer): Count of discovered carousel items
  - `timer` (identifier): Reference pointer for the auto-rotation `setInterval` loop
  - `resumeTimer` (identifier): Timeout pointer managing the manual interaction override window
  - `isHovering` (boolean): Standard mouse pointer boundary detection
  - `manualPauseActive` (boolean): Temporary freeze state triggered by manual navigation
  - `userPausedCarousel` (boolean): Permanent override pause state controlled by the Play/Pause utility
#### Circular Indexing & Normalised Positioning Logic
To render a continuous loop from linear array elements, the rendering engine translates the index of each item relative to the current active pointer (current)
```
let pos = i - current;

if (pos > total / 2)  pos -= total;
if (pos < -total / 2) pos += total;
```
This math projects indices onto a relative range of [-total/2, total/2]. The normalised offset pos then determines class assignments, layout order, pointer activity, and accessibility properties
| Normalised Offset (`pos`) | Applied Class | Transform / Visibility States                   | Pointer Events | Access (`aria-hidden`) |
| ------------------------- | ------------- | ----------------------------------------------- | -------------- | ---------------------- |
| `0`                       | `.main`       | Scale up (`1.15`), brightened, foreground layer | Active         | `false`                |
| `-1`                      | `.prev`       | Positioned left, scale down (`0.78`), dim       | Active         | `true`                 |
| `1`                       | `.next`       | Positioned right, scale down (`0.78`), dim      | Active         | `true`                 |
| `-2`                      | `.far-prev`   | Positioned off-screen left, scaled (`0.55`)     | Disabled       | `true`                 |
| `2`                       | `.far-next`   | Positioned off-screen right, scaled (`0.55`)    | Disabled       | `true`                 |
| Other                     | `.hidden`     | Fully transparent, inactive scale (`0.5`)       | Disabled       | `true`                 |
#### Timer Coordination & User Override Logic
The automatic slider rotation is governed by an asynchronous loop coupled with a temporary override fallback:
 - Standard Loop: Calls `goNext()` every 3,000ms via `setInterval`
 -  Hover Pause: Pauses the timer on cursor entry
 - Crucially, the logic filters input devices by checking `e.pointerType === "mouse"`. Touch-tap interactions are excluded, preventing the slideshow from freezing on mobile devices
 -  Manual Override: Clicking or swiping triggers `pauseForManualNavigation()`, which executes the following sequence:
    - Clears the primary slideshow timer immediately
    - Sets `manualPauseActive = true` and applies a visual style modifier (`.temp-paused`)
    - Clears any active resumeTimer to reset the evaluation window
    - Spawns a new `setTimeout` callback set to 10,000ms. Once that duration expires without further user input, `manualPauseActive` is set to false, and `startTimer()` is executed to safely resume standard automation
#### Roving Tabindex for Control Dots
To provide full keyboard support without cluttering the screen reader focus queue, the indicator navigation implements a roving tabindex pattern:
   - By default, `keyboardDotMode` is set to false, assigning `tabIndex = -1` to all selection dots
   - When a user clicks the carousel play/pause control, `keyboardDotMode` changes to true. The active indicator is assigned `tabIndex = 0` while non-active siblings remain at `tabIndex = -1`
   - A keydown listener captures directional keys (`ArrowRight`, `ArrowLeft`), shifting focus to adjacent siblings, cycling focus to the start or end of the array, and updating the current active slide indicator
### Lightbox & Zoom-Pan Physics Engine
The lightbox is an overlay modal configured as an isolated environment that supports zoom states and touch-pointer panning
```
    Lightbox Close/Zoom/Caption UI Controls (Z-Index: 30)
                           │
            Prev/Next Arrow Controls (Z-Index: 20)
                           │
 ┌─────────────────────────────────────────────────────────┐
 │  Double-Buffered Image Stage (Z-Index: 2 & 3)           │
 │  [Standby Image Container] ──> Preloaded Off-Screen     │
 │  [Active Image Container]  ──> Live Panning Viewport    │
 └─────────────────────────────────────────────────────────┘
                           │
            Backdrop & Dark Overlay (Z-Index: 1)
```
#### Viewport Lock Management
To prevent double-scrolling issues where background document elements scroll while dragging visual assets inside the modal, the window coordinates are locked when opening the viewer:
```
savedScrollY = window.scrollY;

document.body.style.position = "fixed";
document.body.style.top = -${savedScrollY}px;
document.body.style.width = "100%";
```
Upon dismissal, standard document layout states are restored, and the view is scrolled back to the cached position:
```
document.body.style.position = "";
window.scrollTo(0, savedScrollY);
```
#### Double-Buffered Slide Transitions
To ensure animations remain smooth during slide transitions inside the lightbox, the script implements a double-buffered image layout:
 - Off-Screen Prep: When navigating to another slide, the incoming asset is rendered inside a hidden image node (`standbyImage`) and placed off-screen via CSS translation transforms (`.from-right` or `.from-left`)
 -  Framerate Synchronisation: Nested `requestAnimationFrame` hooks ensure style updates register with the browser's paint cycle before introducing transition styles
 -  Active Transition: The current active image (`activeImage`) is translated out of view, and the `standbyImage` moves into the active viewport coordinates (`.center`)
 -  Reference Swap: Once the transitions complete, the DOM object references are swapped:
 ```
    const temp = activeImage;
    activeImage = standbyImage;
    standbyImage = temp;
    standbyImage.src = ""; // Flushes standby memory to free resources
```
#### Intelligent Image Preloading Heuristics
To improve performance and prevent rendering delays, the modal includes a predictive caching queue:
```
const preloadIndexes = [
  (index - 2 + total) % total,
  (index - 1 + total) % total,
  (index + 1) % total,
  (index + 2) % total
];
```
Whenever the viewport index changes, the system calculates a localised buffer window. It instantiates in-memory HTML Image nodes to cache the nearest images before the user navigates to them
#### Dynamic Zoom Transformation Origin
When double-clicking or double-tapping to zoom, scaling from the centre can cause users to lose their point of reference. To fix this, the script calculates the click coordinate offset relative to the image bounds, setting a natural focus origin:
```
\text{OriginX} = \left( \frac{X_{\text{click}} - \text{Rect}_{\text{left}}}{\text{Rect}_{\text{width}}} \right) \times 100

\text{OriginY} = \left( \frac{Y_{\text{click}} - \text{Rect}_{\text{top}}}{\text{Rect}_{\text{height}}} \right) \times 100
```
This math sets the CSS transformOrigin to matching percentage values, ensuring the image scales directly from the user's cursor location
#### Advanced Pointer Drag, Boundary Physics, and Momentum Engine
Once zoomed (`isLightboxZoomed = true`), the system enables a drag-to-pan physics engine managed through unified Pointer Events
```
       [Raw Drag Coordinates]
                 │
                 v
      [Limit Boundaries Check]
        Is offset > maxX/Y ?
             ├── Yes ──> Apply Rubber-Banding Resistance:
             │           Excess travel calculated at 25% scale
             └── No  ──> Apply direct transform values
                 │
                 v
         [Pointer Released]
                 │
                 v
    [Momentum Friction Loop] ──> (Runs via requestAnimationFrame)
      Velocity decays by 6% per frame (multiplier: 0.94)
      Is image close to boundary? 
         ├── Yes ──> Apply extra drag friction (0.75 multiplier)
         └── No  ──> Update coordinates to render frame
                 │
                 v
    [Under Threshold < 0.02] ──> Stop friction loop & ease back to bounds
 ```
 - Real-Time Tracking & Elastic Boundary Rubber-Banding
   - While dragging, the system dynamically calculates maximum panning boundaries based on the current window size and the scaled image dimension (2\times zoom):
    ```
   \text{maxX} = \max\left(0, \frac{\text{Width}_{\text{image}} - \text{Width}_{\text{window}}}{2}\right)
   ```
   - If a user drags past these boundaries, an elastic rubber-banding calculation reduces movement sensitivity to 25%, indicating that the boundary limit has been reached:
   ```
   if (newPanX > maxX) {
   newPanX = maxX + (newPanX - maxX) * 0.25;
   } 
 - Momentum Deceleration Mechanics
   - During drag gestures, the engine calculates real-time pointer speed based on coordinate changes over time:
         ```
         V_x = \frac{\Delta X}{\Delta t}, \quad V_y = \frac{\Delta Y}{\Delta t}
         ```
   - When the pointer is released (`pointerup`), the engine transitions control to a friction-decay loop managed by `requestAnimationFrame`:
     - Friction Multiplier: Each animation frame reduces velocity by 6% to simulate resistance:
    velocityX *= 0.94;
    velocityY *= 0.94;
     - Coordinate Progression: Panning values update on each tick by factoring in velocity scales:
    panX += velocityX * 16;
    panY += velocityY * 16;
     - Edge Friction Control: When moving within 10% of maximum limits, the script applies an extra braking force to prevent abrupt bounces:
    `if (Math.abs(panX) > maxX * 0.9) velocityX *= 0.75;`
     - Loop Termination & Snap-Back: Once the frame velocity drops below a threshold of 0.02, the loop ends. The engine then runs a smooth easing transition to snap the panning coordinates back inside the strict layout bounds (`clampPan`), ensuring the image remains aligned within the viewport
