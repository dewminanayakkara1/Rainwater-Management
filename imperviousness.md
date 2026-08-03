---
layout: default
title: Site Condition Maps
nav_order: 4
---

## Site Condition Maps

These maps show heavy rain hazard, imperviousness, elevation, soil infiltration capacity, and slope across the Horrem study area, giving a general picture of local site conditions relevant to rainwater management.

Use the maps below to:

- Get a rough sense of ground conditions in your part of the study area
- See where soil and slope are more or less favourable for infiltration based measures
- Understand which factors limit or support different rainwater management options nearby

These are static reference maps, not a parcel specific suitability tool. Use the zoom buttons to look more closely at any area.
<style>
  #gallery { max-width: 800px; margin: 0 auto; }
  #galleryViewport { position: relative; width: 100%; height: 500px; overflow: hidden; border: 1px solid #ddd; border-radius: 6px; background: #f4f4f4; display: flex; align-items: center; justify-content: center; }
  #galleryImage { max-width: 100%; max-height: 100%; transition: transform 0.15s ease; cursor: grab; user-select: none; }
  .gallery-nav-btn { position: absolute; top: 50%; transform: translateY(-50%); background: rgba(0,0,0,0.55); color: #fff; border: none; width: 40px; height: 40px; border-radius: 50%; font-size: 1.3em; cursor: pointer; line-height: 1; }
  #prevBtn { left: 10px; }
  #nextBtn { right: 10px; }
  #zoomControls { text-align: center; margin-top: 10px; }
  #zoomControls button { padding: 6px 14px; margin: 0 4px; border: 1px solid #999; border-radius: 6px; background: #fff; cursor: pointer; font-size: 0.95em; }
  #galleryCaption { text-align: center; margin-top: 8px; font-weight: bold; }
  #galleryCounter { text-align: center; color: #666; font-size: 0.9em; }
  #thumbStrip { display: flex; gap: 6px; margin-top: 12px; overflow-x: auto; padding-bottom: 4px; }
  #thumbStrip img { width: 80px; height: 56px; object-fit: cover; border-radius: 4px; cursor: pointer; opacity: 0.55; border: 2px solid transparent; flex-shrink: 0; }
  #thumbStrip img.active { opacity: 1; border-color: #333; }
</style>

<div id="gallery">
  <div id="galleryViewport">
    <button class="gallery-nav-btn" id="prevBtn">&#10094;</button>
    <img id="galleryImage" src="" alt="Map">
    <button class="gallery-nav-btn" id="nextBtn">&#10095;</button>
  </div>
  <div id="galleryCaption"></div>
  <div id="galleryCounter"></div>
  <div id="zoomControls">
    <button id="zoomOutBtn">&minus; Zoom Out</button>
    <button id="zoomResetBtn">Reset</button>
    <button id="zoomInBtn">+ Zoom In</button>
  </div>
  <div id="thumbStrip"></div>
</div>

<script>
  var IMAGES = [
    { src: 'Heavy rain hazard-rare.png', caption: 'Heavy rain hazard-rare' },
    { src: 'Imperviousness.png', caption: 'Imperviousness' },
    { src: 'Elevation.png', caption: 'Elevation' },
    { src: 'Soil infiltartion capacity.png', caption: 'Soil infiltartion capacity' },
    { src: 'slope.png', caption: 'Slope' }
  ];

  var current = 0;
  var zoom = 1;
  var panX = 0;
  var panY = 0;
  var dragging = false;
  var dragStartX = 0;
  var dragStartY = 0;

  var img = document.getElementById('galleryImage');
  var viewport = document.getElementById('galleryViewport');
  var thumbStrip = document.getElementById('thumbStrip');

  IMAGES.forEach(function (item, i) {
    var t = document.createElement('img');
    t.src = item.src;
    t.addEventListener('click', function () { current = i; renderImage(); });
    thumbStrip.appendChild(t);
  });

  function applyTransform() {
    img.style.transform = 'translate(' + panX + 'px,' + panY + 'px) scale(' + zoom + ')';
  }

  function renderImage() {
    img.src = IMAGES[current].src;
    zoom = 1;
    panX = 0;
    panY = 0;
    applyTransform();
    document.getElementById('galleryCaption').innerText = IMAGES[current].caption;
    document.getElementById('galleryCounter').innerText = (current + 1) + ' / ' + IMAGES.length;
    Array.prototype.forEach.call(thumbStrip.children, function (t, i) {
      t.classList.toggle('active', i === current);
    });
  }

  document.getElementById('nextBtn').addEventListener('click', function () {
    current = (current + 1) % IMAGES.length;
    renderImage();
  });

  document.getElementById('prevBtn').addEventListener('click', function () {
    current = (current - 1 + IMAGES.length) % IMAGES.length;
    renderImage();
  });

  document.getElementById('zoomInBtn').addEventListener('click', function () {
    zoom = Math.min(zoom + 1, 10);
    applyTransform();
  });

  document.getElementById('zoomOutBtn').addEventListener('click', function () {
    zoom = Math.max(zoom - 1, 1);
    if (zoom === 1) { panX = 0; panY = 0; }
    applyTransform();
  });

  document.getElementById('zoomResetBtn').addEventListener('click', function () {
    zoom = 1;
    panX = 0;
    panY = 0;
    applyTransform();
  });

  img.addEventListener('mousedown', function (e) {
    if (zoom <= 1) return;
    dragging = true;
    dragStartX = e.clientX - panX;
    dragStartY = e.clientY - panY;
    img.style.cursor = 'grabbing';
  });

  window.addEventListener('mousemove', function (e) {
    if (!dragging) return;
    panX = e.clientX - dragStartX;
    panY = e.clientY - dragStartY;
    applyTransform();
  });

  window.addEventListener('mouseup', function () {
    dragging = false;
    img.style.cursor = 'grab';
  });

  viewport.addEventListener('wheel', function (e) {
    e.preventDefault();
    if (e.deltaY < 0) {
      zoom = Math.min(zoom + 0.5, 10);
    } else {
      zoom = Math.max(zoom - 0.5, 1);
      if (zoom === 1) { panX = 0; panY = 0; }
    }
    applyTransform();
  });

  renderImage();
</script>

[Home](index.html) | [About](about.html) | [Solutions](solutions.html) | [Site Condition Maps](imperviousness.html) | [Rainwater App](form.html)
