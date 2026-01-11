---
title: Mapa do Mundo
---

# Mapa do Mundo

<style>
  /* Remove separador visual do título nesta página */
  h1 {
    border-bottom: none !important;
    margin: 0 0 0.5rem 0;
    padding: 0;
  }

  /* Container do mapa — terá sua altura ajustada por JS */
  #mapa-container {
    width: 100%;
    height: 600px; /* fallback inicial até o JS ajustar */
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
  // CONFIGURAÇÕES (não precisa alterar normalmente)
  const largura = 4096;
  const altura = 4096;
  const IMAGE_URL = 'https://vonhohenheim728-arch.github.io/jujutsu-renkai/04_Fotos/mundo.png';

  // Função que calcula altura ótima para o mapa com base no espaço visível.
  // Garante:
  // - usa o espaço abaixo do título/containers do Quartz
  // - respeita limites mínimo (minH) e máximo (maxH)
  // - evita deixar o container com 0px (que mata a renderização do Leaflet)
  function computeMapHeight(container) {
    const marginBottomSafety = 16; // espaço confortável
    const minH = 400;              // altura mínima aceitável
    const maxH = Math.min(900, Math.floor(window.innerHeight * 0.9)); // teto prático

    // getBoundingClientRect dá a distância do topo da viewport
    const rect = container.getBoundingClientRect();
    const top = rect.top;

    // altura disponível da viewport abaixo do topo do container
    const available = window.innerHeight - top - marginBottomSafety;

    // escolher um valor entre minH e maxH
    const h = Math.max(minH, Math.min(available, maxH));
    return h;
  }

  // Ajuste inicial do container (antes de criar o mapa)
  const container = document.getElementById('mapa-container');
  function adjustContainerHeight() {
    const h = computeMapHeight(container);
    container.style.height = h + 'px';
  }

  // Primeiro ajuste imediato
  adjustContainerHeight();

  // Cria o mapa Leaflet (após o container já ter altura inicial)
  const bounds = [[0, 0], [altura, largura]];

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

  // Função que reajusta o container e força o Leaflet a recalcular
  function resizeAndInvalidate() {
    adjustContainerHeight();
    // invalidateSize garante que o Leaflet redesenhe com a nova caixa
    mapa.invalidateSize();
  }

  // Chamadas de segurança: imediata e com pequeno delay
  resizeAndInvalidate();
  setTimeout(resizeAndInvalidate, 200);

  // Reajusta ao redimensionar a janela (resize)
  let resizeTimer = null;
  window.addEventListener('resize', () => {
    // debounce para evitar chamadas excessivas
    if (resizeTimer) clearTimeout(resizeTimer);
    resizeTimer = setTimeout(() => {
      resizeAndInvalidate();
      resizeTimer = null;
    }, 120);
  });

  // Se seu tema inserir conteúdo depois (ex.: imagens/ads que empurram layout),
  // recalcula também após conteúdo carregar:
  window.addEventListener('load', () => {
    setTimeout(resizeAndInvalidate, 150);
  });

</script>
