<script>
export default {
  props: [
    'jugadores',
    'indiceActual',
    'viendoRol',
    'impostorIndex',
    'palabra',
    'mostrarPista',
    'mostrarCategoria',
  ],
  methods: {
    verRol() {
      this.$emit('verRol');
    },
    siguiente() {
      this.$emit('siguiente');
    },
  },
};
</script>

<template>
  <div class="seccion">
    <template v-if="viendoRol === null">
      <h2>Turno de ver rol</h2>
      <p class="contador">
        Jugador {{ indiceActual + 1 }} de {{ jugadores.length }}
      </p>
      <div class="panelTurno">
        <p>Es el turno de:</p>
        <h3 class="nombreJugador">{{ jugadores[indiceActual] }}</h3>
        <p class="instruccion">Pulsa para ver tu rol en secreto</p>
        <button class="boton botonGrande" @click="verRol()">Ver mi rol</button>
      </div>
    </template>

    <template v-else>
      <div v-if="indiceActual === impostorIndex" class="tarjetaImpostor">
        <h3>IMPOSTOR</h3>
        <p v-if="mostrarCategoria" class="palabraImpostor">
          Categoria: <span>{{ palabra?.categoria }}</span>
        </p>
        <p v-if="mostrarPista" class="palabraImpostor">
          Pista: <span>{{ palabra?.pista }}</span>
        </p>
        <p v-if="!mostrarPista && !mostrarCategoria">
          Sin pistas. Buena suerte.
        </p>
      </div>
      <div v-else class="tarjetaCivil">
        <h3>CIVIL</h3>
        <p class="palabraCivil">
          Tu palabra: <span>{{ palabra?.palabra }}</span>
        </p>
      </div>
      <button
        class="boton botonSiguiente botonGrande botonAncho"
        @click="siguiente()"
      >
        Siguiente
      </button>
    </template>
  </div>
</template>
