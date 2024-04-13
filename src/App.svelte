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

  let datos = []

  onMount(() => {
    d3.csv("./data/clase.csv", d3.autoType).then(data => {
      datos = data
    })
  })
</script>

<main>
  <div class="header">
    <h3 class="headline">
      <b>Jardín Literario</b>
      Preferencias literarias de la clase
    </h3>
    <p class="bajada">Explorando los logros olímpicos a través de datos</p>
  </div>

  <div class="container">
    {#each datos as item}
      <div class="elemento" style="background-color: {colorGenero(item.Genero)}">
        <img src="{obtenerSVG(item.Horas)}" alt="SVG" />
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
