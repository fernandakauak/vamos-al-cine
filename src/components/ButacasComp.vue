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
}
</script>

<template>
    <header>
        <h2>Cine PokeMetálicos <span>presenta</span>:</h2>
        <h1>Carrie</h1>
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
            <h2>Total <span>${{ total }}</span></h2>

            <button :disabled="!puedeComprar" @click="comprar">
              {{ comprado ? '¡Comprado con éxito!' : 'Comprar Entrada' }}
            </button>
        </aside>
        <article>
            <h1>Función Especial: 1 de Agosto</h1>

            <div>Pantalla</div>

            <table class="butacas">
                <thead>
                    <tr>
                        <th></th>
                        <th v-for="num in 8" :key="num">{{ num }}</th>
                    </tr>
                </thead>
                <tbody>
                    <tr v-for="fila in ['A', 'B', 'C', 'D', 'E']" :key="fila">
                        <th>{{ fila }}</th>
                        <td 
                            v-for="num in 8" 
                            :key="num"
                            :class="obtenerClase(`${fila}${num}`)"
                            @click="seleccionar(`${fila}${num}`)"
                        >
                            {{ seleccionadas.includes(`${fila}${num}`) ? `${fila}${num}` : '\u00a0' }}
                        </td>
                    </tr>
                </tbody>
            </table>
        </article>
    </section>
</template>

<style scoped>
    section {
        width: 96%;
        display: flex;
        gap: 2%;
        padding: 2%;
    }

    article {
        width: 76%;
        margin: 0;
        background-color: red;
    }

    aside {
        width: 20%;
        border-radius: 15px;
        background-color: darkslategrey;
        color: white;
        padding: 2%;
    }

    .butacas {
        display: grid;
        grid-template-columns: auto repeat(8, 1fr);
        gap: 5px;
        width: auto;
        max-width: 350px;
        margin: auto;
    }

    .butacas thead, 
    .butacas tbody, 
    .butacas tr {
        display: contents;
    }

    .butacas th {
        display: flex;
        align-items: center;
        justify-content: center;
        min-height: 40px;
    }

    .butacas td {
        display: flex;
        align-items: center;
        justify-content: center;
        aspect-ratio: 1 / 1;
        color: white;
        border-radius: 10px;
        padding: 0;
        box-sizing: border-box;
    }
    
    .butacas td.disponible {
        background-color: blueviolet;
        cursor: pointer;
    }

    .butacas td.seleccionada {
        background-color: green;
        font-weight: bold;
        cursor: pointer;
    }

    .butacas td.ocupada {
        background-color: darkslategray;
        cursor: not-allowed;
    }
</style>