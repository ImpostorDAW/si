<script>
/**
 * componente que gestiona la fase de votacion
 * aqui los jugadores votan a quien creen que es el impostor
 * tambien se muestra el tiempo restante y cuantos civiles e impostores quedan
 */
export default {
  props: ['jugadores', 'impostorIndices', 'votados', 'tiempoRestante'],
  computed: {
    civilesVivos() {
      return (
        this.jugadores.length -
        this.votados.length -
        this.impostorIndices.length
      );
    },
    impostoresVivos() {
      const votadosImpostores = this.votados.filter(i =>
        this.impostorIndices.includes(i)
      ).length;
      return this.impostorIndices.length - votadosImpostores;
    },
  },
  methods: {
    votar(i) {
      this.$emit('votar', i);
    },
    estaVotado(i) {
      return this.votados.includes(i);
    },
  },
};
</script>

<template>
  <div class="votacionFase">
    <h2>¿Quién es el impostor?</h2>
    <p class="contadorTiempo">
      <template v-if="tiempoRestante > 0">
        Tiempo restante: {{ tiempoRestante }}s
      </template>
      <template v-else> ⏰ ¡Es hora de votar! </template>
    </p>
    <p class="contadorJugadores">
      Quedan {{ civilesVivos }} civiles y {{ impostoresVivos }}
      {{ impostoresVivos === 1 ? 'impostor' : 'impostores' }}
    </p>
    <div class="listaVotacion">
      <button
        v-for="(j, i) in jugadores"
        :key="j"
        class="botonVotar"
        @click="votar(i)"
        :disabled="estaVotado(i)"
      >
        {{ j }}
      </button>
    </div>
  </div>
</template>

<style scoped>
.votacionFase {
  background: white;
  padding: 1.5rem;
  border-radius: 12px;
  color: black;
  box-shadow: 0 4px 12px rgba(0, 0, 0, 0.1);
  text-align: center;
}

.votacionFase h2 {
  color: #4a6fa5;
  margin-bottom: 1rem;
  font-size: 1.5rem;
}

.contadorTiempo {
  font-size: 0.9rem;
  color: #757575;
  margin-bottom: 0.5rem;
}

.contadorJugadores {
  font-size: 0.9rem;
  color: #757575;
  margin-bottom: 1rem;
}

.listaVotacion {
  display: flex;
  flex-direction: column;
  gap: 0.5rem;
  margin: 1rem 0;
}

.botonVotar {
  padding: 1rem 1.5rem;
  font-size: 1.1rem;
  border: none;
  border-radius: 8px;
  background: #5bc0de;
  color: white;
  cursor: pointer;
}

.botonVotar:disabled {
  background: #cccccc;
  cursor: not-allowed;
}
</style>
