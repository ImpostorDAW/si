<script>
/**
 * componente que se muestra cuando los jugadores aciertan quien era el impostor
 * muestra el nombre del impostor y la palabra clave, ademas de un boton para reiniciar
 */
export default {
  props: ['jugadores', 'votadoIndice', 'palabra'],
  methods: {
    reiniciar() {
      this.$emit('reiniciar');
    },
  },
};
</script>

<template>
  <div class="finFase">
    <div class="mensajeExito">
      <h2>¡Acertaste!</h2>
      <p>
        <strong>{{ jugadores[votadoIndice] }}</strong> era el impostor
      </p>
    </div>
    <div class="infoPalabra">
      <p>
        <strong>Palabra:</strong>
        <span class="palabraGrande">{{ palabra?.palabra }}</span>
      </p>
      <p><strong>Categoria:</strong> {{ palabra?.categoria }}</p>
      <p><strong>Pista:</strong> {{ palabra?.pista }}</p>
    </div>
    <button class="botonReiniciar" @click="reiniciar()">Nueva partida</button>
  </div>
</template>

<style scoped>
.finFase {
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

.palabraGrande {
  font-size: 1.8rem;
  font-weight: bold;
  color: #4a6fa5;
  display: block;
  margin: 0.5rem 0;
}

.infoPalabra {
  background: #f9f9f9;
  padding: 1rem;
  border-radius: 8px;
  margin: 1rem 0;
  text-align: left;
  font-size: 0.95rem;
  border: 1px solid #e0e0e0;
}

.infoPalabra p {
  margin: 0.3rem 0;
}

.botonReiniciar {
  padding: 0.8rem 1.5rem;
  font-size: 1rem;
  border: none;
  border-radius: 8px;
  background: #d9534f;
  color: white;
  cursor: pointer;
  width: 100%;
  margin-top: 1rem;
}
</style>
