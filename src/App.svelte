<script>
  import * as d3 from "d3"
  import { schemeCategory10 } from "d3-scale-chromatic";


  /* Array donde guardaremos la data */
  let deportistas = []

  /* Escala para edades */
  let grosor = d3.scaleLinear()
  
  /* Escala para genero */
  let colorGenero = d3.scaleOrdinal(["F", "M"], ["pink", "cyan"])

  /* Escala para continentes */
  let colorContinentes = d3.scaleOrdinal(schemeCategory10)
  
  /* Escala para altura */
  let radioAltura = d3.scaleRadial()

  d3.csv("./data/deportistas.csv", d3.autoType).then(data => {
    console.log(data)

    /* Actualizamos dominio y rango con la data de edad */
    let minMaxEdad = d3.extent(data, d => d.edad)
    grosor = grosor.domain(minMaxEdad).range([1, 20])
    
    /* Actualizamos dominio y rango con la data de altura */
    let minMaxAltura = d3.extent(data, d => d.altura)
    radioAltura = radioAltura.domain(minMaxAltura).range([25, 50])


    colorContinentes = colorContinentes.domain(data.map(d => d.continente))
    
    deportistas = data
  })
</script>

<main>
  <h3 class="headline">Deportistas olímpicos</h3>

  <!-- Conedor de las entidades -->
  <div class="container">
    
    <!-- Iteramos la data para visualizar c/ entidad -->
  {#each deportistas as dep}
    <div class="person-container">
      <div class="medal"></div>
      <div class="person" style="border-width: {grosor(dep.edad)}px; background-color:{colorGenero(dep.genero)}; width: {2 * radioAltura(dep.altura)}px; height: {2 * radioAltura(dep.altura)}px; border-color: {colorContinentes(dep.continente)}"></div>
      <p class="name">{dep.nombre}</p>
    </div>
  {/each}

</div>

</main>

<style>
  .headline {
    font-size: 24px;
    text-align: center;
    margin-top: 50px;
  }
  .container {
    display: flex;
    justify-content: center;
    align-items: end;
    margin: auto;
    flex-wrap: wrap;
    max-width: 1000px;
    gap: 30px;
    /* border: 1px solid blue; */
  }
  .person-container {
    display: flex;
    justify-content: center;
    flex-direction: column;
    align-items: center;
    flex: 180px 0 0;
  }
  .person {
    width: 100px;
    height: 100px;
    border: 10px solid black;
    border-radius: 50%;
    box-sizing: border-box;
    background-color: pink;
  }
  .medal {
    width: 15px;
    height: 15px;
    border: 1px solid black;
    border-radius: 50%;
    background-color: gold;
    margin: 5px 0;
  }
  .name {
    font-size: 12px;
    font-weight: bold;
    margin-top: 5px;
  }
</style>
