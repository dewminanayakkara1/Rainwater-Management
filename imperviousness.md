---
layout: default
title: Imperviousness Map
nav_order: 4
---

<div id="map" style="height: 500px; margin-bottom: 1em;"></div>
<div id="solution" style="margin: 1em 0; font-size: 1.1em;"></div>
<link rel="stylesheet" href="https://unpkg.com/leaflet/dist/leaflet.css" />
<script src="https://unpkg.com/leaflet/dist/leaflet.js"></script>
<script>
// Set the map center and zoom for Kerpen region
var map = L.map('map').setView([50.92, 6.70], 14);
L.tileLayer('https://{s}.tile.openstreetmap.org/{z}/{x}/{y}.png', {
  attribution: '© OpenStreetMap contributors'
}).addTo(map);

fetch('https://raw.githubusercontent.com/dewminanayakkara1/Rainwater-Management/refs/heads/main/imp%20new.json')
  .then(response => response.json())
  .then(data => {
    L.geoJSON(data, {
      style: function(feature) {
        var perc = feature.properties['%'];
        let fillColor = perc <= 30 ? 'green'
                       : perc <= 70 ? 'orange'
                       : 'red';
        return {
          color: fillColor,
          fillColor: fillColor,
          weight: 1,
          fillOpacity: 0.5
        };
      },
      onEachFeature: function(feature, layer) {
        var perc = feature.properties['%'];
        var sol = feature.properties['Solution'];
        layer.on('click', function() {
          document.getElementById('solution').innerHTML =
            'Imperviousness: <b>' + perc + '%</b><br>Recommended Solution: <b>' + sol + '</b>';
        });
        layer.bindPopup('Imperviousness: ' + perc + '%<br>Solution: ' + sol);
      }
    }).addTo(map);
  });
</script>



[Home](index.html) | [About](about.html) | [Solutions](solutions.html) | [Impervious Map](imperviousness.html) | [Rainwater App](form.html)
