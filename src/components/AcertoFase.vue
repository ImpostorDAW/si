<script>
/**
 * componente que se muestra cuando los jugadores aciertan que un jugador es impostor
 * pero aun quedan mas impostores por descubrir
 */
export default {
  // atributos que recibe este componente
  props: ['jugadores', 'votadoIndice'],
  methods: {
    /**
     * emite un evento para volver a la pantalla de votacion
     */
    volverVotar() {
      this.$emit('volverVotar');
    },
  },
};
</script>

<template>
  <div class="seccion">
    <div class="panelResultado resultadoExito">
      <h2>¡Es impostor!</h2>
      <p>{{ jugadores[votadoIndice] }} era impostor.</p>
      <p>Aún quedan impostores por encontrar...</p>
    </div>
    <button class="boton botonJugador botonAncho" @click="volverVotar()">
      Continuar votando
    </button>
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

.panelResultado {
  padding: 1.5rem;
  border-radius: 12px;
  color: #fff;
  margin-bottom: 1rem;
  text-align: center;
  background: linear-gradient(135deg, #43e97b, #38f9d7);
}

.resultadoExito {
  background: linear-gradient(135deg, #43e97b, #38f9d7);
  color: #222;
}

.panelResultado h2 {
  margin-bottom: 0.5rem;
  font-size: 1.6rem;
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
  width: 100%;
}

.boton:hover:not(:disabled) {
  background: #3a5a80;
  transform: translateY(-2px);
}

.botonJugador {
  background: #5bc0de;
}

.botonAncho {
  width: 100%;
  margin-top: 1rem;
}
</style>
