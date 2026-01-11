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

  /* Wrapper do mapa com "moldura" uniforme */
  #mapa-wrapper {
    width: 100%;
    height: 80vh;          /* altura total do mapa + moldura */
    padding: 5%;           /* espaço uniforme em todos os lados */
    margin: 1rem 0;
    box-sizing: border-box;
    display: flex;
    justify-content: center; /* centraliza horizontalmente */
    align-items: center;     /* centraliza verticalmente */
    background-color: #fff;  /* cor da moldura */
    border: 2px solid #ccc;  /* opcional: deixa uma borda sutil */
  }

  /* Container do Leaflet ocupando todo o wrapper */
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

  // Ajuste ao redimensionar a janela
  let resizeTimer = null;
  window.addEventListener('resize', () => {
    if (resizeTimer) clearTimeout(resizeTimer);
    resizeTimer = setTimeout(() => {
      mapa.invalidateSize();
      resizeTimer = null;
    }, 120);
  });
</script>
