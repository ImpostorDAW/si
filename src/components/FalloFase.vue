<script>
/**
 * componente que se muestra cuando se falla al votar a un impostor
 * muestra un mensaje de error y un boton para volver a votar
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
  <div class="falloFase">
    <div class="mensajeFallo">
      <h2>No es {{ jugadores[votadoIndice] }}</h2>
      <p>Sigue intentando...</p>
    </div>
    <button class="botonVolver" @click="volverVotar()">Volver a votar</button>
  </div>
</template>

<style scoped>
.falloFase {
  background: white;
  padding: 1.5rem;
  border-radius: 12px;
  color: black;
  box-shadow: 0 4px 12px rgba(0, 0, 0, 0.1);
  text-align: center;
}

.mensajeFallo {
  padding: 1.5rem;
  border-radius: 12px;
  color: white;
  margin-bottom: 1rem;
  background: linear-gradient(135deg, #ff6b6b, #ff8e53);
}

.mensajeFallo h2 {
  margin-bottom: 0.5rem;
  font-size: 1.5rem;
}

.botonVolver {
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
