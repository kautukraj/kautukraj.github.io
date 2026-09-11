---
layout: page
permalink: /travel/
title: Travel
travel_map: true
---

<section class="travel-banner" aria-labelledby="travel-heading">
  <div class="travel-banner__intro">
    <p class="travel-banner__eyebrow">Travel log</p>
    <h2 id="travel-heading">A growing corner of the world</h2>
    <p>Places I have been fortunate enough to visit so far.</p>

    <div class="travel-stats" aria-label="Travel statistics">
      <div class="travel-stat">
        <span class="travel-stat__value" id="countries-visited">41</span>
        <span class="travel-stat__label">of 195 countries</span>
      </div>
      <div class="travel-stat">
        <span class="travel-stat__value" id="countries-percent">21.0%</span>
        <span class="travel-stat__label">of the world's countries</span>
      </div>
      <div class="travel-stat">
        <span class="travel-stat__value" id="land-area-covered">15.4M km&sup2;</span>
        <span class="travel-stat__label">combined land area</span>
      </div>
      <div class="travel-stat">
        <span class="travel-stat__value" id="land-area-percent">10.3%</span>
        <span class="travel-stat__label">of global land area</span>
      </div>
    </div>

    <p class="travel-banner__note">41 countries &middot; 3 continents &middot; more to come</p>
  </div>

  <div class="travel-map-wrap">
    <div id="travel-map" class="travel-map" role="application" aria-label="Zoomable world map highlighting countries Kautuk Raj has visited, with Jamshedpur in India marked as his home town"></div>
    <p class="travel-map__legend">
      <span class="travel-map__legend-dot travel-map__legend-dot--visited" aria-hidden="true"></span> Visited countries
      <span class="travel-map__legend-dot travel-map__legend-dot--home" aria-hidden="true"></span> Home country
    </p>
  </div>
</section>

<script src="https://unpkg.com/leaflet@1.9.4/dist/leaflet.js" integrity="sha512-BwHfrr4c9kmRkLw6iXFdzcdWV/PGkVgiIyIWLLlTSXzWQzxuSg4DiQUCpauz/EWjgk5TYQqX/kvn9pG1NpYfqg==" crossorigin=""></script>
<script>
  (function () {
    var visitedCountries = [
      ['SG', 710], ['TH', 510890], ['PH', 298170], ['ID', 1811570],
      ['QA', 11610], ['FR', 547557], ['CH', 39516], ['MC', 2.02],
      ['BE', 30278], ['NL', 33481], ['ES', 498800], ['LU', 2586],
      ['LI', 160], ['IT', 294140], ['VA', 0.44], ['AT', 82409],
      ['SK', 48105], ['HU', 90530], ['DE', 348560], ['DK', 42430],
      ['SE', 410340], ['GR', 128900], ['AE', 83600], ['MY', 328550],
      ['LK', 64630], ['EG', 995450], ['TR', 769630], ['BH', 786],
      ['JP', 364500], ['KZ', 2699700], ['UZ', 425400], ['AZ', 82658],
      ['OM', 309500], ['KE', 569140], ['TZ', 885800], ['SA', 2149690],
      ['IE', 68890], ['GB', 241930], ['HR', 55960], ['BA', 51130],
      ['ME', 13450]
    ];
    var visitedCountryCodes = visitedCountries.map(function (country) {
      return country[0];
    });
    var totalCountries = 195;
    var worldLandArea = 148940000;
    var totalArea = visitedCountries.reduce(function (sum, country) {
      return sum + country[1];
    }, 0);

    document.getElementById('countries-visited').textContent = visitedCountries.length;
    document.getElementById('countries-percent').textContent =
      ((visitedCountries.length / totalCountries) * 100).toFixed(1) + '%';
    document.getElementById('land-area-covered').textContent =
      (totalArea / 1000000).toFixed(1) + 'M km\u00b2';
    document.getElementById('land-area-percent').textContent =
      ((totalArea / worldLandArea) * 100).toFixed(1) + '%';

    function flagImage(countryCode) {
      if (!countryCode || countryCode.length !== 2 || countryCode === '-99') {
        return '';
      }
      return '<img class="travel-map__flag" src="https://flagcdn.com/20x15/' +
        countryCode.toLowerCase() + '.png" alt=""> ';
    }

    function countryCode(feature) {
      return feature.properties['ISO3166-1-Alpha-2'] || '';
    }

    function countryName(feature) {
      return feature.properties.name || 'Country';
    }

    var map = L.map('travel-map', {
      attributionControl: false,
      minZoom: 1,
      maxZoom: 6,
      scrollWheelZoom: true,
      worldCopyJump: true,
      zoomControl: true
    }).setView([20, 10], 2);

    fetch('https://cdn.jsdelivr.net/gh/datasets/geo-countries@master/data/countries.geojson')
      .then(function (response) {
        if (!response.ok) {
          throw new Error('Unable to load the world map.');
        }
        return response.json();
      })
      .then(function (world) {
        var countries = L.geoJSON(world, {
          style: function (feature) {
            var code = countryCode(feature);
            var fillColor = code === 'IN' ? '#1f638a' :
              visitedCountryCodes.indexOf(code) !== -1 ? '#ce0000' : '#e8e4df';
            return {
              color: '#ffffff',
              weight: 0.55,
              fillColor: fillColor,
              fillOpacity: 1
            };
          },
          onEachFeature: function (feature, layer) {
            var code = countryCode(feature);
            layer.bindTooltip(flagImage(code) + countryName(feature), {
              sticky: true,
              direction: 'top',
              opacity: 0.95
            });
            layer.on({
              mouseover: function (event) {
                event.target.setStyle({weight: 1.5, color: '#6f6861'});
                event.target.bringToFront();
              },
              mouseout: function (event) {
                countries.resetStyle(event.target);
              }
            });
          }
        }).addTo(map);

        map.fitBounds(countries.getBounds(), {padding: [8, 8]});

        L.circleMarker([22.8046, 86.2029], {
          radius: 6,
          color: '#ffffff',
          weight: 2,
          fillColor: '#1f638a',
          fillOpacity: 1
        }).addTo(map).bindTooltip('Jamshedpur', {
          permanent: true,
          direction: 'top',
          offset: [0, -7],
          className: 'travel-home-tooltip'
        });
      });
  }());
</script>
