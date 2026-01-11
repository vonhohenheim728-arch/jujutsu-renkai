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

  /* Ajuste do container principal do Quartz */
  main {
    padding-top: 0 !important; /* remove padding extra acima do conteúdo */
  }

  /* Container do mapa — altura ajustada dinamicamente pelo JS */
  #mapa-container {
    width: 100%;
    height: 600px; /* fallback inicial */
    margin: 0 0 1rem 0;
    overflow: hidden;
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

  const container = document.getElementById('mapa-container');

  function computeMapHeight() {
    const marginBottomSafety = 16;
    const minH = 400;
    const maxH = Math.min(900, Math.floor(window.innerHeight * 0.9));

    const rect = container.getBoundingClientRect();
    const top = rect.top;
    const available = window.innerHeight - top - marginBottomSafety;
    const h = Math.max(minH, Math.min(available, maxH));
    return h;
  }

  function adjustContainerHeight() {
    const h = computeMapHeight();
    container.style.height = h + 'px';
  }

  // Ajuste inicial
  adjustContainerHeight();

  // Criação do mapa Leaflet
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

  function resizeAndInvalidate() {
    adjustContainerHeight();
    mapa.invalidateSize();
  }

  // Ajustes de segurança
  resizeAndInvalidate();
  setTimeout(resizeAndInvalidate, 200);
  window.addEventListener('resize', () => setTimeout(resizeAndInvalidate, 120));
  window.addEventListener('load', () => setTimeout(resizeAndInvalidate, 150));
</script>
