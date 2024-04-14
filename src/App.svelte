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
      ["#000000","#4F4641"], ["#83EA00","#CDFF8E"],["#9F00D7","#CF78ED"], ["#A7850B","#D4B648"], ["#E53619","#FF8975"], ["#3F1A51","#A08CAA"], ["#175873","#6B9CB0"],
      ["#767676","#D4CEBA"], ["#004DB6","#5BA1FF"], ["#B16262","#E19C9C"], ["#6B2B07","#BD7F5B"], ["#00B554","#00FF77"], ["#FF586C","#F8C3C1"],
      ["#FECE2E","#FFE695"], ["#FD6E2A","FFAB84"], ["#FF0099","#FF97D5"], ["#9D2229","#DC7B80"], ["#6ED6FD","#C5EFFF"]
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


  onMount(() => {
    d3.csv("./data/clase.csv", d3.autoType).then(data => {
      datos = data
    })
  })

  // Función para cambiar el color del SVG
  function cambiarColor(genero) {
    return colorGenero(genero);
  }


</script>

<main>
  <div class="header">
  
    <h1 class="headline">
      Jardín Literario
    </h1>
    <p class="bajada">Explorando las preferencias literarias de la clase a través de datos</p>
  </div>

  <div class="container">
    {#each datos as item}
      <div class="elemento">
        <p class="nombre">{item.Nombre}</p>
        {#if obtenerSVGFlor(item.Motivacion)}
          {#if item.Motivacion == "Aprendizaje"}
            <svg width="100" height="100" viewBox="0 0 401 533" fill="none" xmlns="http://www.w3.org/2000/svg">
              <path fill-rule="evenodd" clip-rule="evenodd" d="M135.191 229.658C136.234 233.067 133.058 221.723 128.048 212.16C50.8299 64.722 134.755 -81.2672 184.842 52.9842C199.168 91.3813 225.011 388.457 135.191 229.658Z" fill="{cambiarColor(item.Genero)[1]} "/>
              <path fill-rule="evenodd" clip-rule="evenodd" d="M199.998 250.225C198.541 251.901 199.667 247.388 199.681 247.302C211.465 178.738 317.076 -3.46544 391.499 105.107C455.641 198.67 140.567 366.373 199.681 247.302" fill="{cambiarColor(item.Genero)[1]} "/>
              <path fill-rule="evenodd" clip-rule="evenodd" d="M216.944 278.736C205.96 282.74 268.526 272.323 282.648 275.934C400.585 306.101 450.9 461.163 328.042 439.379C313.851 436.863 127.593 275.711 251.839 274.574" fill="{cambiarColor(item.Genero)[1]} "/>
              <path fill-rule="evenodd" clip-rule="evenodd" d="M208.872 294.806C195.919 294.311 213.607 332.992 215.354 338.835C228.055 381.304 218.851 429.029 209.321 471.161C170.906 640.991 47.0816 416.924 174.816 303.853C183.484 296.177 198.877 290.433 205.623 281.651" fill="{cambiarColor(item.Genero)[1]} "/>
              <path fill-rule="evenodd" clip-rule="evenodd" d="M176.209 309.246C170.225 296.618 155.952 328.744 143.698 335.043C71.3671 372.239 -5.67217 309.558 0.329596 229.294C6.63659 144.953 115.59 226.281 139.099 255.643C149.708 268.89 163.688 280.096 175.972 291.785C177.965 293.68 191.614 308.315 189.135 308.821" fill="{cambiarColor(item.Genero)[1]} "/>
              <path fill-rule="evenodd" clip-rule="evenodd" d="M153.629 262.603C147.584 266.936 129.993 290.328 143.864 317.146C177.272 381.751 253.263 346.556 237.837 279.028C226.843 230.903 169.333 250.043 153.629 262.603Z" fill="{cambiarColor(item.Genero)[0]}"/>
            </svg>
          {/if}
          {#if item.Motivacion == "Entretenimiento"}
            <svg width="100" height="100" viewBox="0 0 417 460" fill="none" xmlns="http://www.w3.org/2000/svg">
              <path fill-rule="evenodd" clip-rule="evenodd" d="M92.2464 120.469C78.3641 70.9254 106.421 19.3339 155.552 4.05512C178.718 -3.15174 203.819 -0.673542 225.131 10.9172C246.447 22.5048 262.168 42.226 268.711 65.5894C269.884 69.7757 270.58 72.2744 270.58 72.2744C270.58 72.2744 270.58 72.2744 270.585 72.2744C290.177 57.8575 314.865 52.1974 338.779 56.6382C362.694 61.0838 383.7 75.2334 396.804 95.728C398.246 97.9832 399.692 100.242 401.133 102.497C429.59 147.003 418.635 205.956 376.088 237.268C417.594 271.838 425.51 332.531 394.255 376.595C394.084 376.832 393.918 377.071 393.749 377.31C379.468 397.441 357.436 410.7 332.962 413.89C308.489 417.081 283.792 409.914 264.827 394.118C264.827 394.118 264.297 395.763 263.378 398.616C255.92 421.721 239.257 440.724 217.32 451.135C195.39 461.545 170.13 462.44 147.515 453.609C98.5677 434.495 72.9712 380.569 89.1087 330.561C90.0276 327.709 90.559 326.063 90.559 326.063C39.3518 324.14 -0.900367 281.593 0.0153173 230.357C0.100718 225.556 0.186119 220.737 0.274682 215.936C0.708012 191.639 10.8976 168.538 28.5502 151.838C46.2012 135.137 69.8335 126.243 94.1173 127.154C94.1173 127.154 93.4167 124.655 92.2464 120.469Z" fill="{cambiarColor(item.Genero)[1]}" />
              <path fill-rule="evenodd" clip-rule="evenodd" d="M179.649 178.454C205.426 156.74 246.102 164.453 270.435 195.664C294.763 226.875 293.593 269.842 267.816 291.556C242.042 313.269 201.365 305.556 177.033 274.345C152.705 243.134 153.876 200.167 179.649 178.454Z" fill="{cambiarColor(item.Genero)[0]}"/>
            </svg>
          {/if}
          {#if item.Motivacion == "Ambas"}
            <svg width="100" height="100" viewBox="0 0 529 502" fill="none" xmlns="http://www.w3.org/2000/svg">
              <path fill-rule="evenodd" clip-rule="evenodd" d="M295.266 203.929C291.721 204.313 303.46 203.322 313.793 200.194C473.098 152.002 600.754 261.821 459.491 285.838C419.088 292.707 122.438 262.368 295.266 203.929Z" fill="{cambiarColor(item.Genero)[0]}"/>
              <path fill-rule="evenodd" clip-rule="evenodd" d="M227.99 178.864C226.533 180.541 227.659 176.027 227.673 175.942C239.457 107.378 345.068 -74.826 419.491 33.7467C483.633 127.309 168.559 295.013 227.673 175.942" fill="{cambiarColor(item.Genero)[0]}"/>
              <path fill-rule="evenodd" clip-rule="evenodd" d="M250.106 275.862C238.468 274.757 299.436 292.245 310.636 301.576C404.153 379.51 382.927 541.143 281.362 468.663C269.631 460.291 170.732 234.722 283.403 287.104" fill="{cambiarColor(item.Genero)[0]}"/>
              <path fill-rule="evenodd" clip-rule="evenodd" d="M226.896 247.098C217.645 238.02 204.714 278.54 202.078 284.039C182.909 324.008 143.997 353.132 108.605 377.898C-34.055 477.729 25.035 228.636 195.61 230.886C207.188 231.036 222.442 237.14 233.341 235.179" fill="{cambiarColor(item.Genero)[0]}"/>
              <path fill-rule="evenodd" clip-rule="evenodd" d="M231.675 193.278C225.691 180.649 211.418 212.776 199.164 219.075C126.833 256.27 49.7939 193.589 55.7957 113.325C62.1027 28.9842 171.056 110.313 194.565 139.675C205.174 152.921 219.154 164.128 231.438 175.816C233.431 177.711 247.08 192.346 244.601 192.852" fill="{cambiarColor(item.Genero)[0]}"/>
              <path fill-rule="evenodd" clip-rule="evenodd" d="M128.684 120.469C114.802 70.9254 142.859 19.3339 191.99 4.05512C215.155 -3.15174 240.257 -0.673542 261.569 10.9172C282.885 22.5048 298.606 42.226 305.149 65.5894C306.322 69.7757 307.018 72.2744 307.018 72.2744C307.018 72.2744 307.018 72.2744 307.023 72.2744C326.614 57.8575 351.303 52.1974 375.217 56.6382C399.132 61.0838 420.138 75.2334 433.242 95.728C434.684 97.9832 436.13 100.242 437.57 102.497C466.028 147.003 455.073 205.956 412.526 237.268C454.032 271.838 461.948 332.531 430.692 376.595C430.522 376.832 430.356 377.071 430.186 377.31C415.905 397.441 393.874 410.7 369.4 413.89C344.926 417.081 320.23 409.914 301.265 394.118C301.265 394.118 300.735 395.763 299.816 398.616C292.358 421.721 275.695 440.724 253.758 451.135C231.828 461.545 206.568 462.44 183.953 453.609C135.005 434.495 109.409 380.569 125.546 330.561C126.465 327.709 126.997 326.063 126.997 326.063C75.7895 324.14 35.5374 281.593 36.4531 230.357C36.5385 225.556 36.6239 220.737 36.7124 215.936C37.1458 191.639 47.3353 168.538 64.9879 151.838C82.639 135.137 106.271 126.243 130.555 127.154C130.555 127.154 129.854 124.655 128.684 120.469Z" fill="{cambiarColor(item.Genero)[1]}"/>
              <path fill-rule="evenodd" clip-rule="evenodd" d="M216.087 178.454C241.863 156.74 282.539 164.453 306.872 195.664C331.2 226.875 330.03 269.842 304.253 291.556C278.48 313.269 237.802 305.556 213.471 274.345C189.143 243.134 190.313 200.167 216.087 178.454Z" fill="{cambiarColor(item.Genero)[0]}"/>
            </svg>
          {/if}
          {#if item.Motivacion == "Ninguno"}
            <svg width="100" height="100" viewBox="0 0 404 392" fill="none" xmlns="http://www.w3.org/2000/svg">
              <path fill-rule="evenodd" clip-rule="evenodd" d="M23.9795 55.4058C25.3517 46.6744 0.226326 120.559 0.00789924 145.761C-0.648964 221.088 39.7252 290.564 96.915 317.959C188.607 361.878 228.08 346.872 275.769 345.163C332.164 343.142 370.905 294.738 392.53 220.832C411.762 155.095 414.03 31.4452 341.249 23.2216C295.288 18.0255 272.041 64.7018 263.901 110.723C263.553 112.686 264.221 112.694 263.901 110.723C255.909 62.046 246.792 -7.0756 177.885 0.589641C150.17 3.67567 152.443 72.3354 145.186 97.5868C141.957 108.816 147.188 109.119 145.186 97.5868C125.663 -14.7092 20.9041 6.25869 5.49706 116.489" fill="{cambiarColor(item.Genero)[1]}"/>
              <mask id="mask0_50_52" style="mask-type:luminance" maskUnits="userSpaceOnUse" x="54" y="309" width="297" height="83">
              <path d="M54 309.003H350.359V391.595H54V309.003Z" fill="white"/>
              </mask>
              <g mask="url(#mask0_50_52)">
              <mask id="mask1_50_52" style="mask-type:luminance" maskUnits="userSpaceOnUse" x="-24" y="42" width="405" height="350">
              <path fill-rule="evenodd" clip-rule="evenodd" d="M0.802382 98.0106C2.17354 89.2807 -22.9311 163.152 -23.1493 188.35C-23.8057 263.664 16.5351 333.128 73.6776 360.518C165.294 404.43 204.734 389.426 252.383 387.718C308.732 385.697 347.44 337.302 369.048 263.408C388.263 197.683 390.53 74.0541 317.81 65.8319C271.886 60.6367 248.659 107.305 240.525 153.319C240.177 155.281 240.845 155.289 240.525 153.319C232.54 104.65 223.431 35.54 154.58 43.2039C126.889 46.2894 129.16 114.937 121.908 140.184C118.682 151.411 123.909 151.715 121.908 140.184C102.402 27.9077 -2.27046 48.872 -17.6647 159.083" fill="white"/>
              </mask>
              <g mask="url(#mask1_50_52)">
              <path fill-rule="evenodd" clip-rule="evenodd" d="M54.3169 343.202C87.2151 275.4 342.955 328.524 347.274 336.968C382.081 405.043 75.8916 442.201 54.3169 343.202Z" fill="{cambiarColor(item.Genero)[0]}"/>
              </g>
              </g>
            </svg>
          {/if}
        {/if}
        <object data="{obtenerSVG(item.Horas)}" type="image/svg+xml" width="70" height="100" aria-label="Tallo SVG">
          <img src="public/images/default.svg" alt="SVG Tallo" />
        </object>
        {#if item.LibroFav != null }
          <p class="libro">{item.LibroFav}</p>
        {:else}
          <p></p>
        {/if}
      </div>
    {/each}
  </div>
</main>
<!--
Código original del svg flor
{#if obtenerSVGFlor(item.Motivacion)}
  <object data="{obtenerSVGFlor(item.Motivacion, item.Genero)}" type="image/svg+xml" width="100" height="100" aria-label="Flor SVG">
    <img src="{obtenerSVGFlor(item.Motivacion, item.Genero)}" alt="SVG Flor" />
  </object>
{/if}

-->
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
  .header{
    text-align: center;
  }
</style>
