<script>
  import * as d3 from "d3"
  import { onMount } from "svelte"
  import atletas from "/src/data/athletes.csv"
  // import atletas from "/src/data/athletes.json"

  console.log("atletas", atletas)

  /* 1. Escala para participaciones (cuantitativo > grosor) */
  const minMaxParticipations = d3.extent(atletas, (d) => d.participations)
  let grosorPartic = d3.scaleLinear()
    .domain(minMaxParticipations).range([2, 18])

  /* 2. Escala para medallas (cuantitativo > diámetro círculo) */
  const maxMedallas = d3.max(atletas, (d) => d.medallas)
  const diamMedallas = d3.scaleRadial().domain([0, maxMedallas]).range([0, 100])

  /* 2. Escala para genero (categórico > color) */
  const colorGenero = d3.scaleOrdinal()
    .domain(["F", "M"])
    .range(["#F7DDBA", "#E4D9F2"])

  /* 3. Escala para continentes (categórico > color)   */
  const colorContinentes = d3
    .scaleOrdinal()
    .domain(["América", "África", "Asia", "Europa", "Oceanía"])
    .range(["#ed334e", "#000000", "#fbb132", "#009fe3", "#00963f"])

</script>

<main>
  <div class="header">
    <img src="/images/logo_referencias.svg" width="190" alt="anillos" />
    <h3 class="headline">
      <b>Los reyes del oro</b>
      Medallas, participaciones y dominio en distintos continentes
    </h3>
    <p class="bajada">
      Los atletas con más medallas olímpicas de oro en los Juegos Olímpicos
    </p>
    <img
      class="referencias"
      src="/images/referencias.svg"
      width="490"
      alt="anillos"
    />
  </div>

    <!-- Conedor de las entidades -->
    <div class="container">
      
      <!-- Iteramos la data para visualizar c/ entidad -->
      {#each atletas as dep}
        <div class="person-container">
          <div
            class="person"
            style="
              border-color: {colorContinentes(dep.continent)};
              background-color:{colorGenero(dep.gender)}; 
              width: {diamMedallas(dep.medallas)}px; 
              height: {diamMedallas(dep.medallas)}px; 
              border-width: {grosorPartic(dep.participations)}px; 
            ">
        </div>
          <p class="nombre">{dep.name}</p>
          <p class="deporte">{dep.sport}</p>
        </div>
      {/each}
      <!-- Fin iteración -->

    </div>
</main>

<style>
  .header {
    display: flex;
    justify-content: center;
    align-items: center;
    flex-direction: column;
    margin-top: 20px;
    margin-bottom: 80px;
  }
  .headline {
    font-size: 40px;
    font-weight: 300;
    line-height: 1.2;
    text-align: center;
    margin: 20px;
  }
  .bajada {
    font-size: 24px;
    font-weight: 300;
    text-align: center;
    margin: 10px;
  }
  .headline b {
    display: block;
  }
  .container {
    display: flex;
    justify-content: center;
    align-items: end;
    margin: auto;
    flex-wrap: wrap;
    max-width: 1020px;
    gap: 30px;
    margin-bottom: 100px;
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
  .nombre {
    font-size: 13px;
    font-weight: bold;
    line-height: 1;
    text-transform: uppercase;
    margin: 0;
    margin-top: 8px;
  }
  .deporte {
    font-size: 14px;
    color: #666;
    margin: 0;
  }
  .referencias {
    margin-top: 50px;
    margin-bottom: 20px;
  }
</style>
