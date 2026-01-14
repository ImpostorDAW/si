<script>
/**
 * componente que se encarga de mostrar el rol de cada jugador
 * se va pasando turno por turno y cada uno puede ver su rol en privado
 * si es impostor o civil, con la palabra correspondiente
 */
export default {
  // atributos que recibe este componente
  props: [
    'jugadores',
    'indiceActual',
    'viendoRol',
    'impostorIndices',
    'palabra',
    'mostrarPista',
    'mostrarCategoria',
  ],
  methods: {
    /**
     * emite un evento para indicar que el jugador quiere ver su rol
     */
    verRol() {
      this.$emit('verRol');
    },

    /**
     * emite un evento para pasar al siguiente jugador
     */
    siguiente() {
      this.$emit('siguiente');
    },
  },
};
</script>

<template>
  <div class="seccion">
    <!-- DEBUG -->
    <p v-if="false">
      DEBUG: viendoRol={{ viendoRol }}, indiceActual={{ indiceActual }},
      indiceInicio={{ indiceInicio }}, horario={{ horario }}
    </p>

    <template v-if="viendoRol !== indiceActual">
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
      <div
        v-if="impostorIndices.includes(indiceActual)"
        class="tarjetaImpostor"
      >
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
        @click="siguiente"
      >
        Siguiente
      </button>
    </template>
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

.contador {
  font-size: 0.9rem;
  color: #757575;
  margin-bottom: 0.5rem;
}

.panelTurno {
  text-align: center;
  padding: 1.5rem;
  background: #f5f5f5;
  border-radius: 12px;
  margin: 1rem 0;
  border: 1px solid #e0e0e0;
}

.nombreJugador {
  font-size: 1.75rem;
  color: #4a6fa5;
  margin: 0.5rem 0;
}

.instruccion {
  color: #757575;
  margin: 0.75rem 0;
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
}

.boton:hover:not(:disabled) {
  background: #3a5a80;
  transform: translateY(-2px);
}

.botonGrande {
  padding: 1rem 1.5rem;
  font-size: 1.1rem;
}

.botonSiguiente {
  background: #5cb85c;
  margin-top: 1rem;
}

.botonAncho {
  width: 100%;
}

.tarjetaImpostor {
  padding: 1.5rem;
  border-radius: 12px;
  color: white;
  margin: 1rem 0;
  text-align: center;
  background: linear-gradient(135deg, #ff6b6b, #ff8e53);
}

.tarjetaCivil {
  padding: 1.5rem;
  border-radius: 12px;
  color: white;
  margin: 1rem 0;
  text-align: center;
  background: linear-gradient(135deg, #43e97b, #38f9d7);
}

.tarjetaImpostor h3,
.tarjetaCivil h3 {
  font-size: 1.5rem;
  margin-bottom: 0.5rem;
}

.palabraImpostor {
  font-size: 1.2rem;
  font-weight: bold;
  margin: 0.5rem 0;
}

.palabraImpostor span {
  font-size: 1.4rem;
  color: #ffd700;
}

.palabraCivil {
  font-size: 1.2rem;
  font-weight: bold;
  margin: 0.5rem 0;
}

.palabraCivil span {
  font-size: 1.4rem;
  color: #ffd700;
}
</style>
