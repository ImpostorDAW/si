<script setup>
defineProps({
  datosPartida: Object,
  indiceActual: Number,
  jugadorActual: Object,
  viendoRol: Object,
  mostrarPista: Boolean,
  mostrarCategoria: Boolean,
});

defineEmits(['verRol', 'siguiente']);
</script>

<template>
  <div class="seccion">
    <template v-if="!viendoRol">
      <h2>Turno de ver rol</h2>
      <p class="contador">
        Jugador {{ indiceActual + 1 }} de {{ datosPartida.jugadores.length }}
      </p>
      <div class="panelTurno">
        <p>Es el turno de:</p>
        <h3 class="nombreJugador">{{ jugadorActual?.nombre }}</h3>
        <p class="instruccion">Pulsa para ver tu rol en secreto</p>
        <button class="boton botonGrande" @click="$emit('verRol')">
          Ver mi rol
        </button>
      </div>
    </template>

    <template v-else>
      <div v-if="viendoRol.esImpostor" class="tarjetaImpostor">
        <h3>IMPOSTOR</h3>
        <p v-if="mostrarCategoria" class="palabraImpostor">
          Categoria: <span>{{ datosPartida.palabra?.categoria }}</span>
        </p>
        <p v-if="mostrarPista" class="palabraImpostor">
          Pista: <span>{{ datosPartida.palabra?.pista }}</span>
        </p>
        <p v-if="!mostrarPista && !mostrarCategoria">
          Sin pistas. Buena suerte.
        </p>
      </div>
      <div v-else class="tarjetaCivil">
        <h3>CIVIL</h3>
        <p class="palabraCivil">
          Tu palabra: <span>{{ datosPartida.palabra?.palabra }}</span>
        </p>
      </div>
      <button
        class="boton botonSiguiente botonGrande botonAncho"
        @click="$emit('siguiente')"
      >
        Siguiente
      </button>
    </template>
  </div>
</template>
