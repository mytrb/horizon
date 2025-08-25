Horizon — Live CSS Sky Gradient for Your Location ☀️🌙🌄
=====================================================

[![Download Release](https://img.shields.io/github/v/release/mytrb/horizon?label=Download%20Release&logo=github&color=2b9348)](https://github.com/mytrb/horizon/releases)

A small tool that renders the current sky at your approximate location as a CSS gradient. Use it in static pages, demos, dashboards, or as a background for widgets. Topics: astro, css, graphics, html-day.

Preview images
--------------
Sunrise example  
![Sunrise gradient preview](https://images.unsplash.com/photo-1502082553048-f009c37129b9?auto=format&fit=crop&w=1200&q=80)

Noon example  
![Clear sky preview](https://images.unsplash.com/photo-1503264116251-35a269479413?auto=format&fit=crop&w=1200&q=80)

Overview
--------
Horizon maps sun position and sky state to a CSS linear-gradient. It uses the browser geolocation API or a fallback coordinate. It computes sun altitude and azimuth, then picks gradient stops for color, brightness, and spread. You get a pure CSS string you can drop into an element style or into a stylesheet.

Key features
------------
- Client-side only. No server required.
- Small, dependency-free code. Use in static sites.
- Simple API: returns a CSS gradient string or applies it to an element.
- Presets for dawn, sunrise, noon, golden hour, sunset, twilight, and night.
- Fine control over color stops, opacity, and gradient direction.
- Works with desktop and mobile browsers that support the Geolocation API.

Who this fits
-------------
- Designers who want accurate sky backgrounds.
- Developers building weather or astronomy widgets.
- Educators showing solar motion and light scattering.
- Any static site that needs a mood-aware background.

Getting started
---------------
1) Download the release file from the Releases page and execute the included index.html or script.  
   Download and run: https://github.com/mytrb/horizon/releases

2) Or clone the repo and open index.html in a browser:
   - git clone https://github.com/mytrb/horizon.git
   - open horizon/index.html in your browser

If you prefer the packaged option, download the release artifact from the releases page linked above and open the included index.html file in a browser. The release file contains built assets and a demo.

Quick use (HTML)
----------------
Drop this snippet into any HTML page to apply the gradient to the full page body:

```html
<script src="horizon.min.js"></script>
<script>
  // horizon.getGradient returns a Promise<string>
  horizon.getGradient().then(cssGradient => {
    document.body.style.background = cssGradient;
  });
</script>
```

API (high level)
----------------
- horizon.getGradient({ lat, lon, time, preset }) -> Promise<string>  
  Returns a CSS linear-gradient string. If lat/lon not provided, the script asks for geolocation.

- horizon.applyTo(element, options) -> Promise<void>  
  Computes gradient and applies it to the element's style.background.

- horizon.presets -> object  
  Preset gradient parameter sets for common sky states.

Options
-------
- lat, lon: Coordinates in decimal degrees. If omitted, the browser will request location.
- time: JavaScript Date instance. Defaults to now.
- preset: 'auto' (default) or one of ['dawn','sunrise','noon','golden','sunset','twilight','night'].
- intensity: 0-1. Scale of color saturation and brightness.
- spread: Controls how wide the color bands appear (CSS gradient stops).

Examples
--------
1) Apply gradient to a card element:

```html
<div id="sky-card" style="height:300px; border-radius:12px;"></div>
<script>
  const el = document.getElementById('sky-card');
  horizon.applyTo(el, { intensity: 0.9 });
</script>
```

2) Get CSS string and log it:

```js
horizon.getGradient({ preset: 'auto' }).then(g => {
  console.log('CSS gradient:', g);
});
```

How it works
------------
1. Get coordinates (geolocation). Use fallback if denied.
2. Compute sun altitude and azimuth for that time and place using simplified solar position math.
3. Map altitude to sky state:
   - altitude < -18°: night
   - -18° <= altitude < -6°: astronomical to nautical twilight
   - -6° <= altitude < 0°: civil twilight
   - 0° <= altitude < 10°: sunrise/sunset transition
   - 10° <= altitude: day
4. Pick color palette and stops for the detected state.
5. Build linear-gradient with direction based on sun azimuth and altitude.
6. Return CSS string like:
   linear-gradient(180deg, rgba(18,26,54,1) 0%, rgba(34,88,192,0.6) 50%, rgba(255,215,125,0.9) 100%)

Color design principles
-----------------------
- Use physically inspired palettes for realism.
- Favor warm tones near the sun for sunrise/sunset.
- Keep the top of the sky desaturated at midday.
- Add thin warm rim near horizon during golden hour.
- Fade to deep blues and near-black at night.

Presets and sample palettes
---------------------------
- dawn: deep indigo -> soft magenta -> pale gold
- sunrise: navy -> orange -> soft yellow
- noon: pale blue -> azure -> bright white
- golden: azure -> warm amber -> deep magenta
- sunset: warm red -> burnt orange -> navy
- twilight: navy -> violet -> almost black
- night: near-black -> deep indigo -> faint glow

Customization tips
------------------
- Use intensity to tune saturation for design needs.
- Use spread to compress or expand color bands.
- For a fixed time of day, pass the time option to generate a stable background.
- Animate transitions by interpolating gradients over time with requestAnimationFrame or CSS transitions.
- For SSR use, precompute the gradient on server using known location.

Accessibility and performance
-----------------------------
- The gradient is pure CSS. It runs fast and uses GPU acceleration for painting.
- For low-power devices, call horizon.getGradient only once and cache the result.
- Ensure contrast for any foreground text. Overlay a semi-opaque mask if needed:
  background: linear-gradient(rgba(0,0,0,0.25), rgba(0,0,0,0.25)), <horizon-gradient>;

Demo and playground
-------------------
Open the demo in the release package. Download the release and execute the demo files from the releases page: https://github.com/mytrb/horizon/releases

Use cases and examples
----------------------
- Weather sites: match the background to current sun position.
- Personal dashboards: a calm day/night background.
- Data visualizations: show time of day context next to charts.
- Art projects: generative backgrounds synchronized to user location.

Integration points
------------------
- CSS frameworks: set a utility class .sky-bg that uses the computed CSS string.
- Static sites: generate inline style on build from a known location.
- Web components: wrap horizon logic in a custom element <horizon-sky>.

Contributing
------------
- Report issues on GitHub.
- Open pull requests with clear tests and short descriptions.
- Keep changes small and focused.
- Add visual tests for gradients when possible.

Changelog and releases
-----------------------
Check the releases page for packaged builds, tags, and notes. Download the release file and execute the included demo or build script. Releases: https://github.com/mytrb/horizon/releases

Badges and topics
-----------------
[![topics](https://img.shields.io/badge/-astro-blue?logo=star&logoColor=white)](#) [![topics](https://img.shields.io/badge/-css-blueviolet?logo=css3&logoColor=white)](#) [![topics](https://img.shields.io/badge/-graphics-green?logo=illustrator&logoColor=white)](#) [![topics](https://img.shields.io/badge/-html--day-orange?logo=html5&logoColor=white)](#)

Files in release
----------------
The release package contains:
- horizon.min.js — runtime library
- index.html — demo page
- examples/ — collection of example integrations
- LICENSE, README.md, CHANGELOG.md

License
-------
MIT — check the LICENSE file in the repo.

Contact
-------
Open an issue on GitHub for bug reports or feature requests. Pull requests welcome.

Short checklist
---------------
- [ ] Download release and execute demo: https://github.com/mytrb/horizon/releases  
- [ ] Try the presets and the API.
- [ ] Use the gradient in your site or component.

Credits and resources
---------------------
- Solar math inspiration: NOAA and NOAA Solar Calculator methods.
- Color palettes: built from physical sky references and photographer palettes (Unsplash).
- Demo photos: Unsplash contributors (used for preview only).