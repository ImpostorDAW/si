<script>
/**
 * componente que se muestra durante la fase de juego
 * aqui los jugadores deben describir la palabra por turnos
 * y al final se puede pasar a la fase de votacion
 */
export default {
  // atributos que recibe este componente
  props: ['orden', 'horario', 'palabra', 'impostorIndices', 'indiceInicio'],
  methods: {
    /**
     * emite un evento para pasar a la fase de votacion
     */
    votar() {
      this.$emit('votar');
    },
  },
};
</script>

<template>
  <div class="seccion">
    <h2>A jugar</h2>
    <p>Describan la palabra por turnos.</p>
    <div class="panelInfo">
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
    <button class="boton botonVotar botonGrande botonAncho" @click="votar">
      Votar
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

.seccion h2 {
  color: #4a6fa5;
  margin-bottom: 1rem;
  font-size: 1.5rem;
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

.panelInfo hr {
  margin: 0.5rem 0;
  border: none;
  height: 1px;
  background: #e0e0e0;
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

.botonVotar {
  background: #f0ad4e;
}

.botonGrande {
  padding: 1rem 1.5rem;
  font-size: 1.1rem;
}

.botonAncho {
  width: 100%;
  margin-top: 1rem;
}
</style>
