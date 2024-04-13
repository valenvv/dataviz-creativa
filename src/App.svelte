<script>
  import * as d3 from "d3"
  import { onMount } from "svelte"

  /* Escala para género literario */
  const colorGenero = d3.scaleOrdinal()
    .domain([
      "Terror", "Informativos", "Fantasía", "Ensayo", "Novela Gráfica", "Misterio", "Policial", 
      "Ninguno", "Ciencia Ficción", "Romance", "Acontecimientos Históricos", "Auto ayuda", "Novelas", 
      "Filosofía", "Realismo mágico", "Ficción", "Biografías", "Non Fiction"
    ])
    .range([
      "#000000", "#83EA00", "#9F00D7", "#A7850B", "#E53619", "#3F1A51", "#175873",
      "#767676", "#004DB6", "#B16262", "#6B2B07", "#00B554", "#FF5569",
      "#FECE2E", "#FD6E2A", "#17FAC3", "#9D2229", "#6ED6FD"
    ])

  /* SVGs de tallos */
  function obtenerSVG(horas) {
    const svgsTallo = {
      0: 'public/images/Tallo 0.svg',
      1: 'public/images/Tallo 1.svg',
      2: 'public/images/Tallo 2.svg',
      3: 'public/images/Tallo 3.svg',
      4: 'public/images/Tallo 4.svg',
      5: 'public/images/Tallo 5.svg',
      7: 'public/images/Tallo 7.svg',
      8: 'public/images/Tallo 8.svg',
      10: 'public/images/Tallo 10.svg',
      14: 'public/images/Tallo 14.svg'
    }

    return svgsTallo[horas] || 'public/images/default.svg';
  }

  /* SVGs de flores */
  const svgsFlor = {
    "Aprendizaje": 'public/images/Flor A.svg',
    "Entretenimiento": 'public/images/Flor E.svg',
    "Ambas": 'public/images/Flor A y E.svg',
    "Ninguno": 'public/images/Flor N.svg',
  }

  /* Función para seleccionar el SVG correspondiente para la flor */
  function obtenerSVGFlor(motivacion) {
    return svgsFlor[motivacion] || '';
  }

  let datos = []
  let florObject; // Variable para el objeto de la flor

  onMount(() => {
    d3.csv("./data/clase.csv", d3.autoType).then(data => {
      datos = data
    })
  })

  // Función para cambiar el color del SVG de la flor
  function cambiarColorSVG(genero) {
    const svgDoc = florObject.contentDocument;
    const paths = svgDoc.querySelectorAll("path");
    paths.forEach(path => {
      path.style.fill = colorGenero(genero);
    });
  }
</script>

<main>
  <div class="header">
  
    <h3 class="headline">
      <b>Triunfos Olímpicos</b>
      Medallas, alturas y continentes
    </h3>
    <p class="bajada">Explorando los logros olímpicos a través de datos</p>
  </div>

  <div class="container">
    {#each datos as item}
      <div class="elemento">
        {#if obtenerSVGFlor(item.Motivacion)}
          <object data="{obtenerSVGFlor(item.Motivacion)}" type="image/svg+xml" width="100" height="100" aria-label="Flor SVG" bind:this={florObject} on:load={() => cambiarColorSVG(item.Genero)}>
            <img src="{obtenerSVGFlor(item.Motivacion)}" alt="SVG Flor" />
          </object>
        {/if}
        <object data="{obtenerSVG(item.Horas)}" type="image/svg+xml" width="70" height="100" aria-label="Tallo SVG">
          <img src="public/images/default.svg" alt="SVG Tallo" />
        </object>
        <p class="nombre">{item.Nombre}</p>
      </div>
    {/each}
  </div>
</main>

<style>
  /* Estilos */
  .container {
    display: flex;
    flex-wrap: wrap;
    justify-content: center;
    gap: 20px;
    padding: 20px;
  }

  .elemento {
    display: flex;
    flex-direction: column;
    align-items: center;
    padding: 20px;
    border-radius: 10px;
    box-shadow: 0 0 10px rgba(0, 0, 0, 0.1);
  }

  .nombre {
    margin-top: 10px;
    font-weight: bold;
  }
</style>
