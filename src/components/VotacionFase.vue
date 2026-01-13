<script>
export default {
  props: ['jugadores', 'impostorIndex', 'votados'],
  methods: {
    votar(i) {
      this.$emit('votar', i);
    },
    estaVotado(i) {
      return i !== this.impostorIndex && this.votados.includes(i);
    },
  },
};
</script>

<template>
  <div class="seccion">
    <h2>Quien es el impostor?</h2>
    <p class="contador">
      Faltan {{ jugadores.length - votados.length - 2 }} votos para que gane el
      impostor
    </p>
    <div class="listaVotacion">
      <button
        v-for="(j, i) in jugadores"
        :key="j"
        class="boton botonJugador botonGrande"
        @click="votar(i)"
        :disabled="estaVotado(i)"
      >
        {{ j }}
      </button>
    </div>
  </div>
</template>
