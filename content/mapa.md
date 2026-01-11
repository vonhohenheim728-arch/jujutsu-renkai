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
  #mapa-container {
    width: 100%;
    height: 80vh; /* ocupa 80% da tela */
    margin: 1rem 0;
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

  // Ajuste dos bounds para eliminar espaço em branco em cima
  // Empurramos a imagem até o topo do container
  const bounds = [[0, 0], [altura, largura]]; // formato original
  const topOffset = 0; // ajuste em pixels se necessário (por exemplo 50 para empurrar a imagem 50px para cima)
  const adjustedBounds = [[-topOffset, 0], [altura - topOffset, largura]];

  const mapa = L.map('mapa-container', {
    crs: L.CRS.Simple,
    minZoom: -2,
    maxZoom: 2,
    zoomControl: true
  });

  // Adiciona a imagem usando bounds ajustado
  L.imageOverlay(IMAGE_URL, adjustedBounds).addTo(mapa);

  // Faz o mapa se ajustar ao bounds
