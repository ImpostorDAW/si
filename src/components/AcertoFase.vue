<script>
/**
 * componente que se muestra cuando los jugadores aciertan que un jugador es impostor
 * pero aun quedan mas impostores por descubrir
 */
export default {
  props: ['jugadores', 'votadoIndice'],
  methods: {
    volverVotar() {
      this.$emit('volverVotar');
    },
  },
};
</script>

<template>
  <div class="acertoFase">
    <div class="mensajeExito">
      <h2>¡Es impostor!</h2>
      <p>{{ jugadores[votadoIndice] }} era impostor.</p>
      <p>Aún quedan impostores por encontrar...</p>
    </div>
    <button class="botonContinuar" @click="volverVotar()">
      Continuar votando
    </button>
  </div>
</template>

<style scoped>
.acertoFase {
  background: white;
  padding: 1.5rem;
  border-radius: 12px;
  color: black;
  box-shadow: 0 4px 12px rgba(0, 0, 0, 0.1);
  text-align: center;
}

.mensajeExito {
  padding: 1.5rem;
  border-radius: 12px;
  color: black;
  margin-bottom: 1rem;
  background: linear-gradient(135deg, #43e97b, #38f9d7);
}

.mensajeExito h2 {
  margin-bottom: 0.5rem;
  font-size: 1.6rem;
}

.botonContinuar {
  padding: 0.8rem 1.5rem;
  font-size: 1rem;
  border: none;
  border-radius: 8px;
  background: #5bc0de;
  color: white;
  cursor: pointer;
  width: 100%;
  margin-top: 1rem;
}
</style>
