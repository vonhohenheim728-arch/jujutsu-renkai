---
title: Mapa do Mundo
---

# Mapa do Mundo

<div
  id="mapa-container"
  style="
    width: 100%;
    height: 600px;
    margin: 0;
  "
></div>




<link
  rel="stylesheet"
  href="https://unpkg.com/leaflet@1.9.4/dist/leaflet.css"
/>

<script src="https://unpkg.com/leaflet@1.9.4/dist/leaflet.js"></script>

<script>
 const largura = 4096
 const altura = 4096

const bounds = [[0, 0], [altura, largura]]

const mapa = L.map('mapa-container', {
  crs: L.CRS.Simple,
  minZoom: -2,
  maxZoom: 2,
  zoomControl: true
})

L.imageOverlay(
  'https://vonhohenheim728-arch.github.io/jujutsu-renkai/04_Fotos/mundo.png',
  bounds
).addTo(mapa)

mapa.fitBounds(bounds)
mapa.setMaxBounds(bounds)
mapa.options.maxBoundsViscosity = 1.0



</script>
