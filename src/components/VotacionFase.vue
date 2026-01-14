<script>
/**
 * componente que gestiona la fase de votacion
 * aqui los jugadores votan a quien creen que es el impostor
 * tambien se muestra el tiempo restante y cuantos civiles e impostores quedan
 */
export default {
  // atributos que recibe este componente
  props: ['jugadores', 'impostorIndices', 'votados', 'tiempoRestante'],
  computed: {
    /**
     * devuelve cuantos civiles todavia estan vivos
     */
    civilesVivos() {
      return (
        this.jugadores.length -
        this.votados.length -
        this.impostorIndices.length
      );
    },

    /**
     * devuelve cuantos impostores todavia estan vivos
     */
    impostoresVivos() {
      const votadosImpostores = this.votados.filter(i =>
        this.impostorIndices.includes(i)
      ).length;
      return this.impostorIndices.length - votadosImpostores;
    },
  },
  methods: {
    /**
     * emite un evento para votar a un jugador
     */
    votar(i) {
      this.$emit('votar', i);
    },

    /**
     * comprueba si un jugador ya ha sido votado
     * los impostores se deshabilitan después de ser votados
     */
    estaVotado(i) {
      // Si ya fue votado, no se puede votar de nuevo
      return this.votados.includes(i);
    },
  },
};
</script>

<template>
  <div class="seccion">
    <h2>¿Quién es el impostor?</h2>
    <p class="contador">
      <template v-if="tiempoRestante > 0">
        Tiempo restante: {{ tiempoRestante }}s
      </template>
      <template v-else> ⏰ ¡Es hora de votar! </template>
    </p>
    <p class="contador">
      Quedan {{ civilesVivos }} civiles y {{ impostoresVivos }}
      {{ impostoresVivos === 1 ? 'impostor' : 'impostores' }}
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

<style scoped>
.seccion {
  background: #ffffff;
  padding: 1.5rem;
  border-radius: 12px;
  color: #333333;
  box-shadow: 0 4px 12px rgba(0, 0, 0, 0.1);
  text-align: center;
}

.seccion h2 {
  color: #4a6fa5;
  margin-bottom: 1rem;
  font-size: 1.5rem;
}

.contador {
  font-size: 0.9rem;
  color: #757575;
  margin-bottom: 0.5rem;
}

.listaVotacion {
  display: flex;
  flex-direction: column;
  gap: 0.5rem;
  margin: 1rem 0;
}

.boton {
  padding: 0.8rem 1.5rem;
  font-size: 1rem;
  border: none;
  border-radius: 8px;
  background: #4a6fa5;
  color: white;
  cursor: pointer;
  transition: background 0.2s, transform 0.1s;
}

.boton:hover:not(:disabled) {
  background: #3a5a80;
  transform: translateY(-2px);
}

.boton:disabled {
  background: #cccccc;
  cursor: not-allowed;
}

.botonJugador {
  background: #5bc0de;
}

.botonGrande {
  padding: 1rem 1.5rem;
  font-size: 1.1rem;
}
</style>
