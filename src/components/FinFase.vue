<script>
/**
 * componente que se muestra cuando los jugadores aciertan quien era el impostor
 * muestra el nombre del impostor y la palabra clave, ademas de un boton para reiniciar
 */
export default {
  // atributos que recibe este componente
  props: ['jugadores', 'votadoIndice', 'palabra'],
  methods: {
    /**
     * emite un evento para reiniciar la partida
     */
    reiniciar() {
      this.$emit('reiniciar');
    },
  },
};
</script>

<template>
  <div class="seccion">
    <div class="panelResultado resultadoExito">
      <h2>¡Acertaste!</h2>
      <p>
        <strong>{{ jugadores[votadoIndice] }}</strong> era el impostor
      </p>
    </div>
    <div class="panelInfo">
      <p>
        <strong>Palabra:</strong>
        <span class="palabraGrande">{{ palabra?.palabra }}</span>
      </p>
      <p><strong>Categoria:</strong> {{ palabra?.categoria }}</p>
      <p><strong>Pista:</strong> {{ palabra?.pista }}</p>
    </div>
    <button class="boton botonEliminar botonAncho" @click="reiniciar()">
      Nueva partida
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
}

.resultadoExito {
  background: linear-gradient(135deg, #43e97b, #38f9d7);
  color: #222;
}

.panelResultado h2 {
  margin-bottom: 0.5rem;
  font-size: 1.6rem;
}

.palabraGrande {
  font-size: 1.8rem;
  font-weight: bold;
  color: #4a6fa5;
  display: block;
  margin: 0.5rem 0;
}

.panelInfo {
  background: #f9f9f9;
  padding: 1rem;
  border-radius: 8px;
  margin: 1rem 0;
  text-align: left;
  font-size: 0.95rem;
  border: 1px solid #e0e0e0;
}

.panelInfo p {
  margin: 0.3rem 0;
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

.botonEliminar {
  background: #d9534f;
}

.botonAncho {
  width: 100%;
  margin-top: 1rem;
}
</style>
