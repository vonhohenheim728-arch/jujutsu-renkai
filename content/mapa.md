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
    height: 80vh; /* altura do mapa */
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

  const bounds = [[0,0],[altura, largura]];

  const mapa = L.map('mapa-container', {
    crs: L.CRS.Simple,
    minZoom: 0,
    maxZoom: 2,
    zoomControl: true,
    center: [altura/2, largura/2], // centraliza o mapa
    zoom: 0
  });

  // Adiciona a imagem
  L.imageOverlay(IMAGE_URL, bounds).addTo(mapa);

  // Ajusta o mapa para cobrir todo o container
  mapa.fitBounds(bounds);

  // Define limites de pan restritos ao container
  mapa.setMaxBounds(bounds);
  mapa.options.maxBoundsViscosity = 1.0;

  // Bloqueia movimentos que revelariam fundo
  mapa.on('movestart', function() {
    mapa.invalidateSize();
  });

  // Força recalculo de tamanho do mapa
  mapa.invalidateSize();
  setTimeout(() => mapa.invalidateSize(), 200);

  // Redimensiona corretamente ao mudar a janela
  let resizeTimer = null;
  window.addEventListener('resize', () => {
    if (resizeTimer) clearTimeout(resizeTimer);
    resizeTimer = setTimeout(() => {
      mapa.invalidateSize();
      mapa.fitBounds(bounds); // garante cobertura total
      resizeTimer = null;
    }, 120);
  });
</script>
