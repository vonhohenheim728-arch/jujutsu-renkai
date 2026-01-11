---
title: Mapa do Mundo
---

<style>
  /* Remove qualquer decoração do título */
  h1 {
    border-bottom: none !important;
    margin: 0 0 0.5rem 0;
    padding: 0;
  }

  /* Wrapper SEM moldura */
  #mapa-wrapper {
    width: 100%;
    height: 80vh;        /* altura do mapa */
    margin: 0;
    padding: 0;
    background: transparent;
  }

  /* Container real do Leaflet */
  #mapa-container {
    width: 100%;
    height: 100%;
    margin: 0;
    padding: 0;
  }

  /* Override DEFINITIVO do Leaflet (mata a linha branca) */
  .leaflet-container {
    background: transparent !important;
    outline: none !important;
    outline-offset: 0 !important;
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

  const bounds = [[0, 0], [altura, largura]];

  const mapa = L.map('mapa-container', {
    crs: L.CRS.Simple,
    minZoom: -2,
    maxZoom: 2,
    zoomControl: true
  });

  // Imagem base
  L.imageOverlay(IMAGE_URL, bounds).addTo(mapa);

  // Ajustes de bounds
  mapa.fitBounds(bounds);
  mapa.setMaxBounds(bounds);
  mapa.options.maxBoundsViscosity = 1.0;

  // Garantia de renderização corretacv
  mapa.invalidateSize();
  setTimeout(() => mapa.invalidateSize(), 200);

  // Ajuste ao redimensionar a janela
  let resizeTimer = null;
  window.addEventListener('resize', () => {
    if (resizeTimer) clearTimeout(resizeTimer);
    resizeTimer = setTimeout(() => {
      mapa.invalidateSize();
      mapa.fitBounds(bounds);
      resizeTimer = null;
    }, 120);
  });
</script>
