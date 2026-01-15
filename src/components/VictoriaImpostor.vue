<script>
/**
 * componente que se muestra cuando los impostores ganan la partida
 * muestra quienes eran los impostores y la palabra clave
 */
export default {
  props: ['jugadores', 'impostorIndices', 'palabra'],
  methods: {
    reiniciar() {
      this.$emit('reiniciar');
    },
  },
};
</script>

<template>
  <div class="victoriaImpostor">
    <div class="mensajeFracaso">
      <h2>¡Los impostores han ganado!</h2>
      <p>Los impostores eran:</p>
      <ul>
        <li v-for="(idx, index) in impostorIndices" :key="index">
          <strong>{{ jugadores[idx] }}</strong>
        </li>
      </ul>
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
.victoriaImpostor {
  background: white;
  padding: 1.5rem;
  border-radius: 12px;
  color: black;
  box-shadow: 0 4px 12px rgba(0, 0, 0, 0.1);
  text-align: center;
}

.mensajeFracaso {
  padding: 1.5rem;
  border-radius: 12px;
  color: white;
  margin-bottom: 1rem;
  background: linear-gradient(135deg, #ff6b6b, #ff8e53);
}

.mensajeFracaso h2 {
  margin-bottom: 0.5rem;
  font-size: 1.6rem;
}

.mensajeFracaso ul {
  list-style-type: none;
  padding: 0;
  margin: 0.5rem 0;
}

.mensajeFracaso li {
  margin: 0.3rem 0;
  font-size: 1.1rem;
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
