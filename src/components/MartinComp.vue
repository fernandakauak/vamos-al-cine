<script setup>
import { ref, reactive, computed, watch } from 'vue';

const cliente = reactive({ nombre: '' });
const tipoFuncion = ref('normal');
const aviso = ref('');
const comprado = ref(false);

const cantidad = ref(0); 
const elegidos = ref([]);
const subtotal = ref(0);
const total = ref(0);

const puedeComprar = computed(() => {
  return cantidad.value > 0 && cliente.nombre.trim() !== '';
});

watch(cantidad, (nuevaCantidad) => {
  if (nuevaCantidad > 6) {
    aviso.value = 'Máximo 6 butacas por compra 🎫';
  } else {
    aviso.value = '';
  }
});

function comprar() {
  if (!puedeComprar.value) return;
  comprado.value = true;
}
</script>

<template>
</template>

<style scoped>
</style>