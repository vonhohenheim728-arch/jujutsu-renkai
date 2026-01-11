---
title: Mapa do Mundo
---

# Mapa do Mundo

<style>
  /* Remove separador visual do título */
  h1 {
    border-bottom: none !important;
    margin: 0 0 0.5rem 0;
    padding: 0;
  }

  /* Container do mapa */
  #mapa-wrapper {
    width: 100%;
    height: 80vh; /* ocupa 80% da viewport */
    margin: 0;
    padding: 0;
    overflow: hidden;
    display: flex;
    align-items: flex-start; /* alinha a imagem ao topo */
  }

  #mapa-container {
    width: 100%;
    height: 100%;
  }
</style>

<div id="mapa-wrapper">
  <div id="mapa-container"></div>
</div>

<link
  rel="stylesheet"
  href="https://unpkg.com/leaflet@1.9.4/dist/leaflet.css"
/>

<script src="https://unpkg.com/leaflet@1.9.4/dist/leaflet.js"></script>

<script>
  const largura = 4096;
  const altura = 4096;
  const IMAGE_URL = 'https://vonhohenheim728-arch.github.io/jujutsu-renkai/04_Fotos/mundo.png';

  const bounds = [[0,0],[altura, largura]];

  const mapa = L.map('mapa-container', {
    crs: L.CRS.Simple,
    minZoom: -2,
    maxZoom: 2,
    zoomControl: true
  });

  L.imageOverlay(IMAGE_URL, bounds).addTo(mapa);
  mapa.fitBounds(bounds);
  mapa.setMaxBounds(bounds);
  mapa.options.maxBoundsViscosity = 1.0;

  // Garante renderização correta
  mapa.invalidateSize();
  setTimeout(() => { mapa.invalidateSize(); }, 200);

  // Ajusta altura ao redimensionar a janela
  let resizeTimer = null;
  window.addEventListener('resize', () => {
    if (resizeTimer) clearTimeout(resizeTimer);
    resizeTimer = setTimeout(() => {
      mapa.invalidateSize();
      resizeTimer = null;
    }, 120);
  });
</script>
