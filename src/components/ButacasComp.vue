<script setup>
import { ref, reactive, computed, watch } from 'vue';

const cliente = reactive({ nombre: '' });
const tipoFuncion = ref('normal');
const aviso = ref('');
const comprado = ref(false);

const cantidad = ref(0); 
const seleccionadas = ref([]);
const ocupadas = ['A4', 'B2', 'D7'];
const subtotal = ref(0);
const total = ref(0);

const puedeComprar = computed(() => {
  return cantidad.value > 0 && cliente.nombre.trim() !== '' && cantidad.value <= 6;
});

watch([seleccionadas, tipoFuncion], () => {
  cantidad.value = seleccionadas.value.length;
  const precioBase = 5000;
  const adicional = tipoFuncion.value === '3d' ? 1500 : 0;
  subtotal.value = cantidad.value * precioBase;
  total.value = cantidad.value * (precioBase + adicional);
}, { deep: true });

function obtenerClase(butaca) {
  if (ocupadas.includes(butaca)) return 'ocupada';
  if (seleccionadas.value.includes(butaca)) return 'seleccionada';
  return 'disponible';
}

function seleccionar(butaca) {
  if (ocupadas.includes(butaca)) return;
  
  const index = seleccionadas.value.indexOf(butaca);
  if (index !== -1) {
    seleccionadas.value.splice(index, 1);
    aviso.value = '';
  } else {
    if (seleccionadas.value.length >= 6) {
      aviso.value = 'Máximo 6 butacas por compra 🎫';
    } else {
      seleccionadas.value.push(butaca);
      aviso.value = '';
    }
  }
}

function comprar() {
  if (!puedeComprar.value) return;
  comprado.value = true;
};

//Reseña Película
const peliFuncion = ref({
    imgPeli: "/src/assets/img/afiche-carrie.png",
    nombrePeli: "Carrie",
    anioPeli : 1976,
    dirPeli : "Brian de Palma",
    genPeli : "Terror/Misterio",
    resenaPeli : "Carrie White, una tímida adolescente que vive con su madre, una fanática religiosa, es objeto de las burlas constantes de sus compañeros de instituto. Cuando, en las duchas del gimnasio, la chica sufre un ataque de histeria al tener su primera menstruación, a una de sus compañeras se le ocurre gastarle una broma macabra durante la fiesta de graduación. Lo que todos ignoran es que Carrie posee poderes telequinésicos. Adaptación de la novela homónima de Stephen King.",
});

</script>

<template>
    <header>
        <h2>Cine PokeMetálicos <span>presenta</span>:</h2>
        <img class="logo" src="../assets/img/logo-carrie.png" alt="Carrie">
    </header>
    <section>
        <aside>
            <h2>Tu compra</h2>
            <p>Butacas seleccionadas: 
              <span>{{ seleccionadas.length > 0 ? seleccionadas.join(', ') : 'Ninguna todavía' }}</span>
            </p>
            <label for="salapeli">Tipo de Función:</label>
            <select v-model="tipoFuncion" id="salapeli">
                <option value="normal">2D - Normal</option>
                <option value="3d">3D — (+$1.500 por butaca)</option>
            </select>
            <label for="retiro">Nombre de quién retira*:</label>
            <input type="text" id="retiro" placeholder="Tu nombre" v-model="cliente.nombre">

            <p class="aviso" v-if="aviso">{{ aviso }}</p>

            <h3>Subtotal <span>${{ subtotal }}</span></h3>
            <h2 class="total">Total <span>${{ total }}</span></h2>

            <button :disabled="!puedeComprar" @click="comprar" class="botoncomprar">
              {{ comprado ? '¡Comprado con éxito!' : 'Comprar Entrada' }}
            </button>
        </aside>
        <article class="mapa-butacas">
            <h1>Función Especial: 1 de Agosto - 20:00hrs</h1>

            <div class="pantalla">Pantalla</div>

            <div class="butacas">
                <div class="th"></div>
                <div class="th" v-for="num in 8" :key="num">{{ num }}</div>
                
                <template v-for="fila in ['A', 'B', 'C', 'D', 'E']" :key="fila">
                    <div class="th">{{ fila }}</div>
                    <div class="td" 
                        v-for="num in 8" 
                        :key="num"
                        :class="obtenerClase(`${fila}${num}`)"
                        @click="seleccionar(`${fila}${num}`)"
                    >
                        {{ fila }}{{ num }}
                    </div>
                </template>
            </div>
        </article>
    </section>
    <div class="info">
        <img id="portada" src="/src/assets/img/afiche-carrie.png" :alt="peliFuncion.nombrePeli">
        <div class="texto" id="sinopsis">
            <h2>{{ peliFuncion.nombrePeli }} ({{ peliFuncion.anioPeli }})</h2>
            <h3>Director: {{ peliFuncion.dirPeli }} / Género: {{ peliFuncion.genPeli }} </h3>
            <p>{{ peliFuncion.resenaPeli }}</p>
        </div>
    </div>

    <footer>
        Hecho por "PokeMetálicos Asociados" para el bootcamp Curso de Desarrollo de Aplicaciones Front-End SENCE 2026 <br>
        <span>If you've got a taste for terror... take Carrie to the prom.</span>
    </footer>
</template>

<style scoped>
    @import url('https://fonts.googleapis.com/css2?family=Amarante&display=swap');

    h1, h2 {
        font-family: "Amarante", serif;
    }

    header h2 {
        color: darkorange;
        text-transform: uppercase;
        background-color: black;
        width: 45%;
        padding: 2%;
        border-radius: 100px;
        margin: auto;
    }

    section {
        width: 96%;
        display: flex;
        flex-wrap: wrap; 
        gap: 2%;
        padding: 2%;
        margin-bottom: 10px;
    }

    article.mapa-butacas {
        flex: 3 1 350px; 
        margin: 0;
        background-color: lightgoldenrodyellow;
        border-radius: 15px;
        padding: 0 1px 1px 1px;
        margin-top: 5px;
    }

    article.mapa-butacas h1 {
        color: darkred;
    }

    aside {
        flex: 1 1 150px; 
        border-radius: 15px;
        background-color: darkred;
        color: white;
        padding: 2%;
        text-align: left;
        margin-top: 5px;
    }

    select, input {
        padding: 2%;
        width: 100%;
        border-radius: 10px;
        margin: 2% 0;
        border: 1px solid gray;
    }

    label {
        font-size: 12px;
    }

    input {
        width: 95%;
    }

    h2.total {
        color: yellow;
        margin: 2%;
        border-top: 1px solid yellow;
        padding: 2%;
    }

    .botoncomprar {
        width: 100%;
        padding: 2%;
        border-radius: 10px;
        border: 1px solid darkorange;
        background-color: darkorange;
        color: white;
        font-size: 14px;
        cursor: pointer;
        transition: all 0.2s ease;
    }

    .botoncomprar:not(:disabled):hover {
        box-shadow:  0 4px 6px rgba(0, 0, 0, 0.4);
    }

    .botoncomprar:disabled {
        background-color: gray;
        border-color: gray;
        cursor: not-allowed;
    }

    .pantalla {
        background-color: darkslategray;
        width: 65%;
        margin: auto;
        padding: 1%;
        border-radius: 0 0 10px 10px;
        color: white;
    }

    .butacas {
        display: grid;
        grid-template-columns: auto repeat(8, minmax(0, 1fr));
        gap: 5px;
        width: 100%;
        max-width: 350px;
        margin: auto;
        padding: 15px;
    }

    .butacas .th {
        display: flex;
        align-items: center;
        justify-content: center;
        font-weight: bold;
    }

    .butacas .td {
        display: flex;
        align-items: center;
        justify-content: center;
        aspect-ratio: 1 / 1;
        color: transparent;
        border-radius: 10px;
        padding: 0;
        box-sizing: border-box;
    }
    
    .butacas .td.disponible {
        background-color: lightgrey;
        cursor: pointer;
        transition: all 0.2s ease;
    }

    .butacas .td.disponible:hover {
        box-shadow: inset 0 4px 6px rgba(0, 0, 0, 0.4);
        filter: brightness(0.85);
        color: rgba(0, 0, 0, 0.7);
    }

    .butacas .td.seleccionada {
        background-color: orangered;
        color: white;
        font-weight: bold;
        cursor: pointer;
        transition: all 0.2s ease;
    }

    .butacas .td.ocupada {
        background-color: darkslategray;
        cursor: not-allowed;
    }

    .logo {
        width: 40%;
    }

    footer {
        padding: 2%;
        width: 96%;
        margin-top: 5%;
        color: orangered;
        background-color: black;
        border-top: 2px solid yellow;
    }

    footer span {
        font-style: italic;
        font-weight: bold;
    }


    .info {
        display: flex;
        flex-wrap: wrap;
        background-color: black;
        border: 1px solid yellow;
        color: white;
        margin: 10px;
    }

    #portada {
        flex: 1 1 260px; 
        width: 100%;
        max-width: 350px;
        height: auto;
        padding: 20px;
        box-sizing: border-box;
        object-fit: contain;
    }

    #sinopsis {
        flex: 999 1 300px;
        text-align: right;
        padding: 20px;
        box-sizing: border-box;
    }

    #sinopsis p {
        text-wrap: pretty;
    }
    
    #sinopsis h2 {
        color: yellow;
    }
    
</style>