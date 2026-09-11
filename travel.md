---
layout: page
permalink: /travel/
title: Travel
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
    <div id="travel-map" class="travel-map" role="img" aria-label="World map highlighting 41 countries Kautuk Raj has visited"></div>
    <span class="travel-map__home-marker" aria-label="Jamshedpur, India — home town">
      <span class="travel-map__home-marker-label">Jamshedpur</span>
    </span>
    <p class="travel-map__legend">
      <span class="travel-map__legend-dot travel-map__legend-dot--visited" aria-hidden="true"></span> Visited countries
      <span class="travel-map__legend-dot travel-map__legend-dot--home" aria-hidden="true"></span> Home country
    </p>
  </div>
</section>

<script src="https://www.gstatic.com/charts/loader.js"></script>
<script>
  (function () {
    var visitedCountries = [
      ['Singapore', 710], ['Thailand', 510890], ['Philippines', 298170],
      ['Indonesia', 1811570], ['Qatar', 11610], ['France', 547557],
      ['Switzerland', 39516], ['Monaco', 2.02], ['Belgium', 30278],
      ['Netherlands', 33481], ['Spain', 498800], ['Luxembourg', 2586],
      ['Liechtenstein', 160], ['Italy', 294140], ['Vatican City', 0.44],
      ['Austria', 82409], ['Slovakia', 48105], ['Hungary', 90530],
      ['Germany', 348560], ['Denmark', 42430], ['Sweden', 410340],
      ['Greece', 128900], ['United Arab Emirates', 83600], ['Malaysia', 328550],
      ['Sri Lanka', 64630], ['Egypt', 995450], ['Turkey', 769630],
      ['Bahrain', 786], ['Japan', 364500], ['Kazakhstan', 2699700],
      ['Uzbekistan', 425400], ['Azerbaijan', 82658], ['Oman', 309500],
      ['Kenya', 569140], ['Tanzania', 885800], ['Saudi Arabia', 2149690],
      ['Ireland', 68890], ['United Kingdom', 241930], ['Croatia', 55960],
      ['Bosnia and Herzegovina', 51130], ['Montenegro', 13450]
    ];
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

    function drawTravelMap() {
      var chartData = [['Country', 'Visited']];
      visitedCountries.forEach(function (country) {
        chartData.push([country[0], 1]);
      });
      chartData.push(['India', 2]);

      var chart = new google.visualization.GeoChart(document.getElementById('travel-map'));
      chart.draw(google.visualization.arrayToDataTable(chartData), {
        backgroundColor: 'transparent',
        colorAxis: {values: [1, 2], colors: ['#ce0000', '#1f638a']},
        datalessRegionColor: '#e8e4df',
        defaultColor: '#e8e4df',
        displayMode: 'regions',
        enableRegionInteractivity: true,
        keepAspectRatio: true,
        legend: 'none',
        magnifyingGlass: {enable: false},
        region: 'world',
        resolution: 'countries',
        tooltip: {textStyle: {fontName: 'Open Sans', fontSize: 13}}
      });
    }

    google.charts.load('current', {packages: ['geochart']});
    google.charts.setOnLoadCallback(drawTravelMap);
    window.addEventListener('resize', drawTravelMap);
  }());
</script>
