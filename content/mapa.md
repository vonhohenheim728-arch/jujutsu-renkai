---
title: Mapa do Mundo
---

# Mapa do Mundo

<style>
  h1 {
    border-bottom: none !important;
    margin: 0 0 0.5rem 0;
    padding: 0;
  }

  #mapa-container {
    width: 100%;
    height: 80vh; /* container ocupa 80% da tela */
    margin: 1rem 0;
  }
</style>

<div id="mapa-container"></div>

<link
  rel="stylesheet"
  href="https://unpkg.com/leaflet@1.9.4/dist/leaflet.css"
/>

<script src="https://unpkg.com/leaflet@1.9.4/dist/leaflet.js"></script>

<script>
  const largura = 4096;
  const altura = 4096;
  const IMAGE_URL = 'https://vonhohenheim728-arch.github.io/jujutsu-renkai/04_Fotos/mundo.png';

  // Bounds reais da imagem
  const bounds = [[0,0],[altura, largura]];

  const mapa = L.map('mapa-container', {
    crs: L.CRS.Simple,
    minZoom: -2,
    maxZoom: 2,
    zoomControl: true
  });

  // Adiciona a imagem
  L.imageOverlay(IMAGE_URL, bounds).addTo(mapa);

  // Ajusta mapa para caber nos bounds
  mapa.fitBounds(bounds);

  // Define limites de pan para que o usuário não consiga mover mapa além da imagem
  mapa.setMaxBounds(bounds);
  mapa.options.maxBoundsViscosity = 1.0;

  // Força recalculo de tamanho para Leaflet
  mapa.invalidateSize();
  setTimeout(() => mapa.invalidateSize(), 200);

  // Recalcula tamanho ao redimensionar a janela
  let resizeTimer = null;
  window.addEventListener('resize', () => {
    if (resizeTimer) clearTimeout(resizeTimer);
    resizeTimer = setTimeout(() => {
      mapa.invalidateSize();
      resizeTimer = null;
    }, 120);
  });
</script>
