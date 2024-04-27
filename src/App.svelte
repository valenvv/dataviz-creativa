<script>
  import * as d3 from "d3"
  import { onMount } from "svelte"
  const colorGenero = d3.scaleOrdinal()
  .domain(["Fantasía", "No Ficción", "Ninguno", "Ciencia Ficción", 
  "Romance", "Misterio"])
    .range([["#9F00D7","#CF78ED"], ["#E53619","#FF8975"],["#767676","#D4CEBA"], ["#004DB6","#5BA1FF"],
     ["#FF586C","#FF8B87"],  ["#00B554","#00FF77"]])
    .unknown(["#FECE2E","#FFE695"])

  // Define una función de dominio personalizada para asignar "Otros" a cualquier género no especificado
  /* SVGs de tallos */
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
  function obtenerSVG(horas) {

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
  
  // Función para cambiar el color del SVG flor
  function cambiarColor(genero) {
    return colorGenero(genero);
  }


  /* SVGs centro flor*/
  const svgsCentro = {
    "Digital":'public/images/fLectD.svg',
    "Fisico": 'public/images/fLectF.svg',
    "Ambas": 'public/images/fLectA.svg',
  }
  function obtenerSVGCentro(formato) { // ver reutilizar esta funcion
    return svgsCentro[formato] || '';
  }

  /* SVGs centro flor*/
  const svgsImportancia = {
    "Si":'public/images/mariposas.svg',
  }
  function obtenerSVGMariposas(formato) {
    return svgsImportancia[formato] || null;
  }

  let datos = []
  let filtroGenero = '';
  let filtroMotivacion = '';
  let filtroImportancia = '';

  function setFilter(value) {
    if (value == ''){
      datosFiltrados = datos;
    }else{
      filtroGenero = value; // Set the filter to the new value
    }
  }

  // Función para filtrar los datos según los criterios seleccionados
  function filtrarDatos() {
    // Inicialmente, todos los datos están presentes
    let datosFiltrados = datos;

    // Filtrar por género literario
    if (filtroGenero !== '') {
      datosFiltrados = datosFiltrados.filter(item => item.Genero === filtroGenero);
    }

    // Filtrar por motivación
    if (filtroMotivacion !== '') {
      datosFiltrados = datosFiltrados.filter(item => item.Motivacion === filtroMotivacion);
    }

    // Filtrar por importancia
    if (filtroImportancia !== '') {
      datosFiltrados = datosFiltrados.filter(item => item.Importancia === filtroImportancia);
    }

    return datosFiltrados;
  }


  $: datosFiltrados = filtrarDatos();

  onMount(async () => {
    datos = await d3.csv("./data/clase.csv", d3.autoType);
    datosFiltrados = filtrarDatos(); // Update filtered data after the data has loaded
  });
  
  $: filtroGenero && (datosFiltrados = filtrarDatos());
  $: filtroMotivacion && (datosFiltrados = filtrarDatos());
  //$: filtroImportancia && (datosFiltrados = filtrarDatos());

  //FONDO
  const gradient = document.querySelector(".gradient");

  function onMouseMove(event) {
    gradient.style.backgroundImage = 'radial-gradient(at ' + event.clientX + 'px ' + event.clientY+ 'px, #ff8095 0, #f8efe7 70%)';
  }
    
  document.addEventListener("mousemove", onMouseMove);



</script>

<main>
  <div class="header">
  
    <h1 class="headline">
      Jardín Literario
    </h1>
    <p class="bajada">Explorando las preferencias literarias de la clase a través de datos</p>
  </div>
  <!-- Controles de filtro por género  -->
  <div class="filtro">
    <select bind:value={filtroGenero} on:change={(e) => setFilter(filtroGenero, e.target.value)}>
      <option value="">Todos los géneros</option>
      {#each colorGenero.domain() as genero}
        <option value={genero}>{genero}</option>
      {/each}
    </select>
   
  </div>
  
  <!-- <span class="boton-ref" style="background-image: url('public/images/fLectA.svg');"></span> -->
  <a href="#abajo" class="boton-ref-link">
    <img class="boton-ref" src="public\images\iconRefs.png" width="100px" height="100px" alt="">
  </a>
  
  <!-- Controles de filtro por motivación -->
  <select bind:value={filtroMotivacion} on:change={(e) => setFilter(filtroMotivacion, e.target.value)}>
    <option value="">Todas las motivaciones</option>
    {#each Array.from(new Set(datos.map(d => d.Motivacion))) as motivacion}
      <option value={motivacion}>{motivacion}</option>
    {/each}
  </select>
  
  <div class="container">
    {#each datosFiltrados as item}
      <div class="elemento">
        <p class="nombre">{item.Nombre}</p>
          {#if item.Importancia == "Si"}
            <div>
                <img class="mariposas"src="public/images/mariposas.svg" alt="SVG Mariposas" width="40" />
            </div>
          {/if}
          <object  class="centro" data="{obtenerSVGCentro(item.Formato)}" type="image/svg+xml" width="38" height="36" aria-label="Centro SVG">
            <img class="{item.Formato}"src="{obtenerSVGCentro(item.Formato)}" alt="SVG Centro"  />
          </object>
          {#if obtenerSVGFlor(item.Motivacion)}
            {#if item.Motivacion == "Aprendizaje"}
              <svg class="flor"  height="110" viewBox="0 0 417 460" fill="none" xmlns="http://www.w3.org/2000/svg">
                <path fill-rule="evenodd" clip-rule="evenodd" d="M141.16 180.557C142.247 183.238 138.934 174.319 133.705 166.801C53.1255 50.8829 140.704 -63.8957 192.972 41.6545C207.921 71.8428 234.89 305.408 141.16 180.557Z" fill="{cambiarColor(item.Genero)[1]}"/>
                <path fill-rule="evenodd" clip-rule="evenodd" d="M208.379 191.222C206.861 192.898 208.034 188.385 208.049 188.299C220.327 119.735 330.363 -62.4685 407.905 46.1042C474.735 139.667 146.457 307.37 208.049 188.299" fill="{cambiarColor(item.Genero)[1]}"/>
                <path fill-rule="evenodd" clip-rule="evenodd" d="M226.035 219.733C214.591 223.737 279.778 213.32 294.493 216.931C417.372 247.098 469.795 402.16 341.789 380.376C327.003 377.86 132.939 216.708 262.393 215.571" fill="{cambiarColor(item.Genero)[1]}"/>
                <path fill-rule="evenodd" clip-rule="evenodd" d="M217.968 235.423C204.361 234.955 222.942 271.483 224.778 277C238.121 317.105 228.451 362.173 218.44 401.96C178.084 562.336 48.0011 350.742 182.191 243.967C191.297 236.717 207.468 231.293 214.555 223" fill="{cambiarColor(item.Genero)[1]}"/>
                <path fill-rule="evenodd" clip-rule="evenodd" d="M183.593 250.243C177.358 237.615 162.487 269.741 149.72 276.04C74.3577 313.236 -5.90986 250.555 0.343408 170.291C6.9147 85.9497 120.434 167.278 144.928 196.64C155.981 209.887 170.548 221.093 183.346 232.782C185.422 234.676 199.644 249.312 197.06 249.818" fill="{cambiarColor(item.Genero)[1]}"/>
                <path fill-rule="evenodd" clip-rule="evenodd" d="M162.665 172.33C189.522 150.616 231.903 158.329 257.255 189.539C282.603 220.75 281.383 263.718 254.526 285.432C227.673 307.144 185.291 299.431 159.94 268.22C134.592 237.01 135.811 194.042 162.665 172.33Z" fill="{cambiarColor(item.Genero)[0]}"/>
                </svg>
            {/if}
            {#if item.Motivacion == "Entretenimiento"}
              <svg class="flor"  height="110" viewBox="0 0 417 460" fill="none" xmlns="http://www.w3.org/2000/svg">
                <path fill-rule="evenodd" clip-rule="evenodd" d="M92.2464 120.469C78.3641 70.9254 106.421 19.3339 155.552 4.05512C178.718 -3.15174 203.819 -0.673542 225.131 10.9172C246.447 22.5048 262.168 42.226 268.711 65.5894C269.884 69.7757 270.58 72.2744 270.58 72.2744C270.58 72.2744 270.58 72.2744 270.585 72.2744C290.177 57.8575 314.865 52.1974 338.779 56.6382C362.694 61.0838 383.7 75.2334 396.804 95.728C398.246 97.9832 399.692 100.242 401.133 102.497C429.59 147.003 418.635 205.956 376.088 237.268C417.594 271.838 425.51 332.531 394.255 376.595C394.084 376.832 393.918 377.071 393.749 377.31C379.468 397.441 357.436 410.7 332.962 413.89C308.489 417.081 283.792 409.914 264.827 394.118C264.827 394.118 264.297 395.763 263.378 398.616C255.92 421.721 239.257 440.724 217.32 451.135C195.39 461.545 170.13 462.44 147.515 453.609C98.5677 434.495 72.9712 380.569 89.1087 330.561C90.0276 327.709 90.559 326.063 90.559 326.063C39.3518 324.14 -0.900367 281.593 0.0153173 230.357C0.100718 225.556 0.186119 220.737 0.274682 215.936C0.708012 191.639 10.8976 168.538 28.5502 151.838C46.2012 135.137 69.8335 126.243 94.1173 127.154C94.1173 127.154 93.4167 124.655 92.2464 120.469Z" fill="{cambiarColor(item.Genero)[1]}" />
                <path fill-rule="evenodd" clip-rule="evenodd" d="M179.649 178.454C205.426 156.74 246.102 164.453 270.435 195.664C294.763 226.875 293.593 269.842 267.816 291.556C242.042 313.269 201.365 305.556 177.033 274.345C152.705 243.134 153.876 200.167 179.649 178.454Z" fill="{cambiarColor(item.Genero)[0]}"/>
              </svg>
            {/if}
            {#if item.Motivacion == "Ambas"}
              <!-- Flor - AP Y ENT -->
              <svg class="flor"  height="110" viewBox="0 0 529 460" fill="none" xmlns="http://www.w3.org/2000/svg">
                <path fill-rule="evenodd" clip-rule="evenodd" d="M295.266 203.926C291.721 204.31 303.46 203.319 313.793 200.191C473.098 151.999 600.754 261.818 459.491 285.835C419.088 292.704 122.438 262.365 295.266 203.926Z" fill="{cambiarColor(item.Genero)[0]}"/>
                <path fill-rule="evenodd" clip-rule="evenodd" d="M227.748 181.932C226.291 183.469 227.418 179.332 227.432 179.254C239.221 116.412 344.878 -50.5833 419.334 48.9271C483.504 134.68 168.291 288.386 227.432 179.254" fill="{cambiarColor(item.Genero)[0]}"/>
                <path fill-rule="evenodd" clip-rule="evenodd" d="M249.095 274.598C239.644 273.783 289.051 287.719 298.074 295.338C373.42 358.968 354.783 492.777 272.849 433.725C263.386 426.904 184.929 241.265 276.063 283.607" fill="{cambiarColor(item.Genero)[0]}"/>
                <path fill-rule="evenodd" clip-rule="evenodd" d="M226.896 247.095C217.645 238.017 204.714 278.537 202.078 284.036C182.909 324.005 143.997 353.129 108.605 377.895C-34.055 477.726 25.035 228.633 195.61 230.883C207.188 231.033 222.442 237.137 233.341 235.176" fill="{cambiarColor(item.Genero)[0]}"/>
                <path fill-rule="evenodd" clip-rule="evenodd" d="M231.675 193.275C225.691 180.646 211.418 212.773 199.164 219.072C126.833 256.267 49.7939 193.586 55.7957 113.322C62.1027 28.9812 171.056 110.309 194.565 139.671C205.174 152.918 219.154 164.125 231.438 175.813C233.431 177.708 247.08 192.343 244.601 192.849" fill="{cambiarColor(item.Genero)[0]}"/>
                <path fill-rule="evenodd" clip-rule="evenodd" d="M128.684 120.466C114.802 70.9224 142.859 19.3309 191.99 4.05207C215.155 -3.15479 240.257 -0.676594 261.569 10.9142C282.885 22.5018 298.606 42.223 305.149 65.5864C306.322 69.7726 307.018 72.2714 307.018 72.2714C307.018 72.2714 307.018 72.2714 307.023 72.2714C326.614 57.8545 351.303 52.1943 375.217 56.6351C399.132 61.0807 420.138 75.2303 433.242 95.7249C434.684 97.9801 436.13 100.239 437.57 102.494C466.028 147 455.073 205.953 412.526 237.265C454.032 271.835 461.948 332.528 430.692 376.592C430.522 376.829 430.356 377.068 430.186 377.307C415.905 397.438 393.874 410.697 369.4 413.887C344.926 417.078 320.23 409.911 301.265 394.115C301.265 394.115 300.735 395.76 299.816 398.613C292.358 421.718 275.695 440.721 253.758 451.132C231.828 461.542 206.568 462.437 183.953 453.606C135.005 434.492 109.409 380.566 125.546 330.558C126.465 327.706 126.997 326.06 126.997 326.06C75.7895 324.137 35.5374 281.59 36.4531 230.354C36.5385 225.553 36.6239 220.734 36.7124 215.933C37.1458 191.636 47.3353 168.535 64.9879 151.835C82.639 135.134 106.271 126.24 130.555 127.151C130.555 127.151 129.854 124.652 128.684 120.466Z" fill="{cambiarColor(item.Genero)[1]}"/>
                <path fill-rule="evenodd" clip-rule="evenodd" d="M216.087 178.451C241.863 156.737 282.539 164.45 306.872 195.661C331.2 226.872 330.03 269.839 304.253 291.553C278.48 313.265 237.802 305.553 213.471 274.342C189.143 243.131 190.313 200.163 216.087 178.451Z" fill="{cambiarColor(item.Genero)[0]}"/>
                </svg>
                
            {/if}
            {#if item.Motivacion == "Ninguno"}
              <!-- Flor - Nada -->
              <svg class="flor" height="110" viewBox="0 0 404 460" fill="none" xmlns="http://www.w3.org/2000/svg">
                <path fill-rule="evenodd" clip-rule="evenodd" d="M23.9795 65.0812C25.3517 54.8245 0.226326 141.616 0.00789924 171.221C-0.648964 259.707 39.7252 341.32 96.915 373.5C188.607 425.092 228.08 407.464 275.769 405.457C332.164 403.083 370.905 346.223 392.53 259.406C411.762 182.185 414.03 36.9348 341.249 27.2746C295.288 21.1708 272.041 76.0011 263.901 130.062C264.221 132.377 263.553 132.368 263.901 130.062C255.909 72.8814 246.792 -8.31524 177.885 0.689062C150.17 4.31419 152.443 84.9683 145.186 114.631C147.188 128.178 141.957 127.821 145.186 114.631C125.664 -17.2824 20.9041 7.34845 5.49706 136.835" fill="{cambiarColor(item.Genero)[1]}"/>
                <mask id="mask0_50_52" style="mask-type:luminance" maskUnits="userSpaceOnUse" x="54" y="362" width="297" height="98">
                <path d="M54 362.98H350.359V460H54V362.98Z" fill="white"/>
                </mask>
                <g mask="url(#mask0_50_52)">
                <mask id="mask1_50_52" style="mask-type:luminance" maskUnits="userSpaceOnUse" x="-24" y="50" width="405" height="410">
                <path fill-rule="evenodd" clip-rule="evenodd" d="M0.802381 115.129C2.17354 104.874 -22.9311 191.65 -23.1493 221.25C-23.8057 309.721 16.5351 391.319 73.6776 423.494C165.294 475.077 204.734 457.452 252.383 455.446C308.732 453.072 347.44 396.222 369.048 309.42C388.263 232.213 390.53 86.9873 317.81 77.3287C271.886 71.226 248.659 126.047 240.525 180.099C240.845 182.413 240.177 182.404 240.525 180.099C232.54 122.928 223.431 41.745 154.58 50.7478C126.889 54.3723 129.16 135.012 121.908 164.67C123.909 178.215 118.682 177.858 121.908 164.67C102.402 32.7794 -2.27046 57.406 -17.6647 186.87" fill="white"/>
                </mask>
                <g mask="url(#mask1_50_52)">
                <path fill-rule="evenodd" clip-rule="evenodd" d="M54.3169 403.153C87.2151 323.507 342.955 385.911 347.274 395.83C382.081 475.798 75.8916 519.446 54.3169 403.153Z" fill="{cambiarColor(item.Genero)[0]}"/>
                </g>
                </g>
                </svg>
            {/if}
          {/if}
        
        <div class="tallo">
          <object class="tallo" data="{obtenerSVG(item.Horas)}" type="image/svg+xml" height="90" aria-label="Tallo SVG">
            <img src="public/images/default.svg" alt="SVG Tallo" />
          </object>
        </div>
        {#if item.LibroFav != null }
          <p class="libro">{item.LibroFav}</p>
        {:else}
          <p>:(</p>
        {/if}
      </div>
      
    {/each}
  </div>
  <div class="r-gradient"></div>
  <h2 id="abajo">Referencias</h2>
  <div class="referencias">
    <div class="r-genero">
      <p class="r-titlulo">Género de lectura de preferencia:</p>
      <ul>

        <li><span style="background-color: #9F00D7;"></span>Fantasía</li>
        <li><span style="background-color: #E53619;"></span>No Ficción</li>
        <li><span style="background-color: #767676;"></span>Ninguno</li>
        <li><span style="background-color: #004DB6;"></span>Ciencia Ficción</li>
        <li><span style="background-color: #FF586C;"></span>Romance</li>
        <li><span style="background-color: #00B554;"></span>Misterio</li>
        <li><span style="background-color: #FECE2E;"></span>Otros</li>
        
      </ul>
    </div>
    <div class="r-mariposas">
      <p class="r-titlulo">Importancia de la lectura en el desarrollo personal:</p>
      <ul>
        <li><img src="public/images/mariposas.svg" alt="SVG Mariposas" width="30">Sí</li>
        <li><span class="mariposa-no"></span>No</li>
      </ul>
    </div>
    <div class="r-tallo">
      <p class="r-titlulo">Horas de lectura semanal promedio:</p>
      <ul>
        {#each Object.keys(svgsTallo) as svg}
          <li><img src="{obtenerSVG(svg)}" alt="{svg}" height="100">{svg}</li>
        {/each} 
      </ul>
    </div>
    <div class="r-flores">
      <p class="r-titlulo">Motivación de lectura:</p>
      <ul>
        <li><img src="public/images/Flor E.svg" alt="Entretenimiento">Entretenimiento</li>
        <li><img src="public/images/FLOR - APRENDIZAJE.svg" alt="Aprendizaje">Aprendizaje</li>
        <li><img src="public/images/FLOR - AP Y ENT.svg" alt="Ambos">Ambos</li>
        <li><img src="public/images/FLOR - NADA.svg" alt="Ninguno">Ninguno</li>
      </ul>
    </div>
    <div class="r-centros">
      <p class="r-titlulo">Formato de lectura de preferencia:</p>
      <ul>
        <li><span class="r-centro" style="background-image: url('public/images/fLectD.svg');"></span>Digital</li>
        <li><span class="r-centro" style="background-image: url('public/images/fLectF.svg');"></span>Físico</li>
        <li><span class="r-centro" style="background-image: url('public/images/fLectA.svg');"></span>Ambos</li>
        <li><span class="r-centro"></span>Ninguno</li>
      </ul>
    </div>
  </div>
  <footer class="footer" width="100%">
    <div class="waves">
      <div class="wave" id="wave1"></div>
      <div class="wave" id="wave2"></div>
      <div class="wave" id="wave3"></div>
      <div class="wave" id="wave4"></div>
    </div>
    <ul class="social-icon">
      <li class="social-icon__item"><a class="social-icon__link" href="#">
          <ion-icon name="logo-facebook"></ion-icon>
        </a></li>
      <li class="social-icon__item"><a class="social-icon__link" href="#">
          <ion-icon name="logo-twitter"></ion-icon>
        </a></li>
      <li class="social-icon__item"><a class="social-icon__link" href="#">
          <ion-icon name="logo-linkedin"></ion-icon>
        </a></li>
      <li class="social-icon__item"><a class="social-icon__link" href="#">
          <ion-icon name="logo-instagram"></ion-icon>
        </a></li>
    </ul>
    <ul class="menu">
      <li class="menu__item"><a class="menu__link" href="#">Home</a></li>
      <li class="menu__item"><a class="menu__link" href="#">About</a></li>
      <li class="menu__item"><a class="menu__link" href="#">Services</a></li>
      <li class="menu__item"><a class="menu__link" href="#">Team</a></li>
      <li class="menu__item"><a class="menu__link" href="#">Contact</a></li>

    </ul>
    <p>&copy;2021 Nadine Coelho | All Rights Reserved</p>
  </footer>
  <script type="module" src="https://unpkg.com/ionicons@5.5.2/dist/ionicons/ionicons.esm.js"></script>
  <script nomodule src="https://unpkg.com/ionicons@5.5.2/dist/ionicons/ionicons.js"></script>
</main>


<style>
  /* Estilos */

  .headline{
    font-family: "DM Sans";
  }
  .container {
    display: flex;
    flex-wrap: wrap;
    justify-content: center;
    gap: 20px;
    padding: 20px;
    max-width: 1200px;
    position: relative;
    margin-right: 15px;
    margin-left: 15px;
  }

  .elemento {
    flex-grow: 1;
    background-color: #ffe0c785;
    width: 250px;
    max-width: 250px;
    position: relative;
    display: flex;
    flex-direction: column;
    align-items: center;
    padding: 20px;
    border-radius: 10px;
    box-shadow: 0 0 10px rgba(0, 0, 0, 0.1);
    margin-right: 15px;
    margin-left: 15px;
    margin-bottom: 30px;
    transition: all 0.4s ease;
  }

  .elemento:hover {
    transform: scale(1.04);
    /* box-shadow: 0 0 100px #cc25577e; */
  }

  .nombre {
    margin-top: 20px;
    margin-bottom: 40px;
    font-weight:bolder;
    font-size: larger;
  }
  .header{
    text-align: center;
  }

  .centro{
    position: absolute;
    transform: translate(6%, 352%);
  }

  .Entretenimiento{
    position: absolute;
    top: 33%;
    left: 52%;
    transform: translate(-50%, -50%);
  }

  .mariposas{
    position: absolute;
    right: 65%;
    top: 26%;
  }

  .libro{
    text-align: center;
   position: relative;
  }

  /*REFERENCIAS*/

  .referencias {
    position:relative; /*SI CAMBIAMOS ESTO SE SITÚAN AL COSTADO DE LAS FLORES, VER*/
    margin: 0 auto;
    display: flex;
    flex-wrap: wrap;
    justify-content: center;
    gap: 30px;
    padding: 20px;
    margin-right: 15px;
    margin-left: 15px;
    margin-bottom: 200px;
  }

  .r-titlulo{
    font-weight: bold;
  }


  /* RECUADRO DE CADA REFERENCIA */
  .r-mariposas ,.r-tallo,  .r-flores, .r-centros, .r-flores, .r-genero {
    flex-grow: 1;
    flex-direction: column;
    background-color: rgba(255, 255, 255, 0.9);
    border-radius: 10px;
    padding: 10px;
    box-shadow: 0 2px 4px rgba(0, 0, 0, 0.1);
    margin-top: 20px;
    margin-left: 5px;
    align-items: center;
    width: 220px;
    max-width: 220px;
    position: relative;
    padding:20px;

  }

  /*LISTAS DE CADA REFERENCIA*/
  .r-mariposas ul, .r-flores ul, .r-centros ul, .r-flores ul, .r-genero ul{
    list-style: none;
    padding: 0;
    margin: 0;
  }
  .r-tallo ul {
    list-style: none;
    padding: 0;
    margin: 0;
    display: flex;
    flex-wrap: wrap;
    justify-content: space-between;
  }

  .r-genero li, .r-mariposas li, .r-flores li, .r-centros li, .r-flores li{
    display: flex;
    margin-bottom: 5px;
    margin-left: 5px;
    margin-right: 5px;

  }
  .r-tallo li {
    display: flex;
    align-items: center;
    margin-bottom: 5px;
    flex-basis: calc(50% - 5px);
  }

  /* Género */

  .r-genero li span {
    display: inline-block;
    width: 15px;
    height: 15px;
    margin-right: 10%;
    border-radius: 50%;
  }

  /* Desarrollo personal */
  .r-mariposas li {
    width: 20px;
    height: 20px;
    margin-right: 10px;
  }

  /* Horas semanales de lectura */
  .r-tallo li img {
    height: 40px;
    margin-right: 10px;
  }

  /* Motivación */
  .r-flores li img {
    width: 20px;
    height: auto;
    margin-right: 10px;
  }

  .mariposa-no {
    margin-right: 30px;
  }

  /*Formatos de lectura*/
  .r-centros li .r-centro {
    width: 20px;
    height: 20px;
    background-size: contain;
    background-repeat: no-repeat;
    margin-right: 10px;
  }

  /* Boton estático para ir a las referencias */
  .boton-ref{
    display: inline-block;
    padding: 20px;
    
    cursor:pointer;
    position:fixed;
    bottom: 0;
    right: 0;
    z-index: 2;
  }
  
  .boton-ref-link:target .boton-ref {
    display: none;
  }

 /*Filtros*/

  .filtro {
  position: relative;
  display: inline-block;
  }

  .filtro-icono {
    position: absolute;
    right: 10px;
    top: 50%;
    transform: translateY(-50%);
  }

  select {
    padding-right: 25px; /* espacio para el icono */
    z-index: 1;
  }

  /* ANILLADOS */
  /* Borde estilo anillado */
  .referencias .r-mariposas::before,
  .referencias .r-tallo::before,
  .referencias .r-flores::before,
  .referencias .r-centros::before,
  .referencias .r-genero::before {
    content: '';
    position: absolute;
    top: -10px;
    left: 2%;
    width: 96%;
    height: 23px;
    background: url("public/images/anillado.svg");
    background-size: 5%;
    z-index: 2; /* Para colocar el borde detrás del contenido */
  }

  .r-mariposas,
  .r-tallo,
  .r-flores,
  .r-centros,
  .r-genero {
    position: relative;
  }
  

  /* FOOTER! */
  @import url("https://fonts.googleapis.com/css2?family=Poppins:wght@200;300;400;500;600;700;800;900&display=swap");


  .footer {
    position: relative;
    width: 100%;
    height: 300px;
    background: #CC2557;
    min-width:100vh;
    display: flex;
    justify-content: center;
    align-items: center;
    flex-direction: column;
  }

  .social-icon,
  .menu {
    position: relative;
    display: flex;
    justify-content: center;
    align-items: center;
    margin: 10px 0;
    flex-wrap: wrap;
  }

  .social-icon__item,
  .menu__item {
    list-style: none;
  }

  .social-icon__link {
    font-size: 2rem;
    color: #fff;
    margin: 0 10px;
    display: inline-block;
    transition: 0.5s;
  }
  .social-icon__link:hover {
    transform: translateY(-10px);
  }

  .menu__link {
    font-size: 1.2rem;
    color: #fff;
    margin: 0 10px;
    display: inline-block;
    transition: 0.5s;
    text-decoration: none;
    opacity: 0.75;
    font-weight: 300;
  }

  .menu__link:hover {
    opacity: 1;
  }

  .footer p {
    color: #fff;
    margin: 15px 0 10px 0;
    font-size: 1rem;
    font-weight: 300
    
;
  }

  /* en figma */

  .wave { 
    position: absolute;
    top: -100px;
    left: 0;
    width: 100%;
    height: 100px;
    background-image: url("public/images/wave.png");
    background-color: CC2557;
    background-size: 1000px 100px;
  }

  .wave#wave1 {
    z-index: 1000;
    opacity: 1;
    bottom: -1;
    animation: animateWaves 8s linear infinite;
  }

  .wave#wave2 {
    z-index: 999;
    opacity: 0.5;
    bottom: 10px;
    animation: animate 8s linear infinite !important;
  }

  .wave#wave3 {
    z-index: 1000;
    opacity: 0.2;
    bottom: 15px;
    animation: animateWaves 7s linear infinite;
  }

  .wave#wave4 {
    z-index: 999;
    opacity: 0.7;
    bottom: 20px;
    animation: animate 7s linear infinite;
  }

  @keyframes animateWaves {
    0% {
      background-position-x: 1000px;
    }
    100% {
      background-positon-x: 0px;
    }
  }

  @keyframes animate {
    0% {
      background-position-x: -1000px;
    }
    100% {
      background-positon-x: 0px;
    }
  }
 

</style>
