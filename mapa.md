---
title: Mapa mundial
nav_order: 6
description: "Mapa interactivo con los escritores más celebrados de cada país."
permalink: /mapa.html
---

# Mapa mundial de escritores
{: .no_toc }

Haz clic en cualquier marcador para ver los escritores y las obras más celebrados de ese país, por época.
{: .fs-6 .fw-300 }

<div class="map-filter" style="margin-bottom: 1rem;">
  <label for="era-select" style="font-weight: 600; margin-right: .5rem;">Filtrar por época:</label>
  <select id="era-select" style="padding: .3rem .6rem; font-size: 1rem;">
    <option value="all">Todas las épocas</option>
    <option value="clasico">Clásicos</option>
    <option value="siglo19">Siglo XIX</option>
    <option value="siglo20">Siglo XX</option>
    <option value="siglo21">Siglo XXI</option>
  </select>
</div>

<div id="writers-map" style="width: 100%; height: 600px; border-radius: 6px; border: 1px solid #d0d7de;"></div>

<link rel="stylesheet" href="https://unpkg.com/leaflet@1.9.4/dist/leaflet.css"
  integrity="sha256-p4NxAoJBhIIN+hmNHrzRCf9tD/miZyoHS5obTRR9BMY=" crossorigin="" />
<script src="https://unpkg.com/leaflet@1.9.4/dist/leaflet.js"
  integrity="sha256-20nQCchB9co0qIjJZRGuk2/Z9VM+kNiyxNV1lvTlZBo=" crossorigin=""></script>

<script>
  var writersData = {{ site.data.writers | jsonify }};

  var eraLabels = {
    clasico: "Clásico",
    siglo19: "Siglo XIX",
    siglo20: "Siglo XX",
    siglo21: "Siglo XXI"
  };

  document.addEventListener("DOMContentLoaded", function () {
    var map = L.map('writers-map', { worldCopyJump: true }).setView([20, 10], 2);

    L.tileLayer('https://{s}.tile.openstreetmap.org/{z}/{x}/{y}.png', {
      maxZoom: 18,
      attribution: '&copy; <a href="https://www.openstreetmap.org/copyright">OpenStreetMap</a> contributors'
    }).addTo(map);

    function buildPopup(country, era) {
      var html = '<div style="min-width:220px;"><strong>' + country.country + '</strong><br/>';
      var eras = era === 'all' ? ['clasico', 'siglo19', 'siglo20', 'siglo21'] : [era];
      var any = false;
      eras.forEach(function (key) {
        var w = country.writers[key];
        html += '<div style="margin-top:6px;"><em>' + eraLabels[key] + ':</em><br/>';
        if (w && w.author) {
          any = true;
          html += w.author + ' — <em>' + w.work + '</em> (' + w.year + ')';
        } else {
          html += '<span style="color:#888;">Sin figura de consenso reconocida internacionalmente</span>';
        }
        html += '</div>';
      });
      html += '</div>';
      return html;
    }

    var markers = [];

    writersData.forEach(function (country) {
      if (!country.lat || !country.lng) return;
      var marker = L.circleMarker([country.lat, country.lng], {
        radius: 7,
        color: '#7253ed',
        fillColor: '#7253ed',
        fillOpacity: 0.75,
        weight: 1
      }).addTo(map);
      marker.bindPopup(buildPopup(country, 'all'));
      marker._countryData = country;
      markers.push(marker);
    });

    document.getElementById('era-select').addEventListener('change', function (e) {
      var era = e.target.value;
      markers.forEach(function (marker) {
        var country = marker._countryData;
        var hasEntry = era === 'all' || (country.writers[era] && country.writers[era].author);
        var el = marker.getElement ? marker.getElement() : null;
        marker.setStyle({ opacity: hasEntry ? 1 : 0.15, fillOpacity: hasEntry ? 0.75 : 0.1 });
        marker.setPopupContent(buildPopup(country, era));
      });
    });
  });
</script>

---

**{{ site.data.writers | size }} países** representados en este mapa. Los datos son los mismos que en las páginas de [Clásicos](/docs/clasicos.html), [Siglo XIX](/docs/siglo-19.html), [Siglo XX](/docs/siglo-20.html) y [Siglo XXI](/docs/siglo-21.html).
