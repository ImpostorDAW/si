<script>
/**
 * componente que se muestra durante la fase de juego
 * aqui los jugadores deben describir la palabra por turnos
 * y al final se puede pasar a la fase de votacion
 */
export default {
  props: ['orden', 'horario', 'palabra', 'impostorIndices', 'indiceInicio'],
  methods: {
    votar() {
      this.$emit('votar');
    },
  },
};
</script>

<template>
  <div class="juegoFase">
    <h2>A jugar</h2>
    <p>Describan la palabra por turnos.</p>
    <div class="infoJuego">
      <p>
        <strong>Empieza:</strong> {{ orden[indiceInicio] || 'Cargando...' }}
      </p>
      <p>
        <strong>Sentido:</strong>
        {{ horario ? 'Horario' : 'Antihorario' }}
      </p>
      <hr />
      <p>
        <em
          >Los civiles conocen la palabra, los impostores deben adivinarla.</em
        >
      </p>
    </div>
    <button class="botonVotar" @click="votar">Votar</button>
  </div>
</template>

<style scoped>
.juegoFase {
  background: white;
  padding: 1.5rem;
  border-radius: 12px;
  color: black;
  box-shadow: 0 4px 12px rgba(0, 0, 0, 0.1);
  text-align: center;
}

.juegoFase h2 {
  color: #4a6fa5;
  margin-bottom: 1rem;
  font-size: 1.5rem;
}

.infoJuego {
  background: #f9f9f9;
  padding: 1rem;
  border-radius: 8px;
  margin: 1rem 0;
  text-align: left;
  font-size: 0.95rem;
  border: 1px solid #e0e0e0;
}

.infoJuego p {
  margin: 0.3rem 0;
}

.infoJuego hr {
  margin: 0.5rem 0;
  border: none;
  height: 1px;
  background: #e0e0e0;
}

.botonVotar {
  padding: 1rem 1.5rem;
  font-size: 1.1rem;
  border: none;
  border-radius: 8px;
  background: #f0ad4e;
  color: white;
  cursor: pointer;
  width: 100%;
  margin-top: 1rem;
}
</style>
