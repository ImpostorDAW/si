<script setup>
const props = defineProps({
  jugadores: Array,
  jugadoresBloqueados: Array,
});
defineEmits(['votar']);
</script>

<template>
  <div class="seccion">
    <h2>Quien es el impostor?</h2>
    <div class="listaVotacion">
      <button
        v-for="j in jugadores"
        :key="j.nombre"
        class="boton botonJugador botonGrande"
        :disabled="
          jugadoresBloqueados && jugadoresBloqueados.includes(j.nombre)
        "
        @click="$emit('votar', j)"
      >
        {{ j.nombre }}
      </button>
    </div>
  </div>
</template>
