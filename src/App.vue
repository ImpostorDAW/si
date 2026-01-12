<script setup>
import { computed, ref } from 'vue';
import './components/estilos.css';
import { palabraAleatoria } from './data/palabras.js';

import ConfigFase from './components/ConfigFase.vue';
import FalloFase from './components/FalloFase.vue';
import FinFase from './components/FinFase.vue';
import JuegoFase from './components/JuegoFase.vue';
import RolesFase from './components/RolesFase.vue';
import VotacionFase from './components/VotacionFase.vue';

const fase = ref('config');
const jugadores = ref([]);
const nombreInput = ref('');
const editIndex = ref(null);
const mostrarPista = ref(true);
const mostrarCategoria = ref(true);
const categoriaId = ref('');

const datosPartida = ref({
  palabra: null,
  jugadores: [],
  orden: [],
  horario: true,
});
const indiceActual = ref(0);
const viendoRol = ref(null);
const votado = ref(null);

const jugadorActual = computed(
  () => datosPartida.value.jugadores[indiceActual.value]
);
const todosVieron = computed(
  () => indiceActual.value >= datosPartida.value.jugadores.length
);

const agregarJugador = () => {
  const nombre = nombreInput.value.trim();
  if (nombre && !jugadores.value.includes(nombre)) {
    jugadores.value.push(nombre);
    nombreInput.value = '';
  }
};

const eliminar = i => jugadores.value.splice(i, 1);

const editar = i => {
  editIndex.value = i;
  nombreInput.value = jugadores.value[i];
};

const guardarEdicion = () => {
  if (nombreInput.value.trim() && editIndex.value !== null) {
    jugadores.value[editIndex.value] = nombreInput.value.trim();
    cancelarEdicion();
  }
};

const cancelarEdicion = () => {
  editIndex.value = null;
  nombreInput.value = '';
};

const empezar = () => {
  if (jugadores.value.length < 3) return;

  const impostorIndex = Math.floor(Math.random() * jugadores.value.length);
  const listaJugadores = [];
  for (let i = 0; i < jugadores.value.length; i++) {
    listaJugadores.push({
      nombre: jugadores.value[i],
      esImpostor: i === impostorIndex,
      vioRol: false,
    });
  }

  const ordenAleatorio = jugadores.value.slice();
  ordenAleatorio.sort(() => Math.random() - 0.5);

  datosPartida.value = {
    palabra: palabraAleatoria(categoriaId.value || null),
    jugadores: listaJugadores,
    orden: ordenAleatorio,
    horario: Math.random() < 0.5,
  };
  indiceActual.value = 0;
  fase.value = 'roles';
};

const verRol = () => (viendoRol.value = jugadorActual.value);

const siguienteJugador = () => {
  if (viendoRol.value) viendoRol.value.vioRol = true;
  viendoRol.value = null;
  indiceActual.value++;
  if (todosVieron.value) fase.value = 'juego';
};

const votar = jugador => {
  votado.value = jugador;
  fase.value = jugador.esImpostor ? 'fin' : 'fallo';
};

const reiniciar = () => {
  fase.value = 'config';
  datosPartida.value = {
    palabra: null,
    jugadores: [],
    orden: [],
    horario: true,
  };
  viendoRol.value = null;
  indiceActual.value = 0;
  votado.value = null;
};
</script>

<template>
  <div class="juego">
    <h1>Juego del Impostor</h1>

    <ConfigFase
      v-if="fase === 'config'"
      :jugadores="jugadores"
      :editIndex="editIndex"
      :nombreInput="nombreInput"
      :mostrarPista="mostrarPista"
      :mostrarCategoria="mostrarCategoria"
      :categoriaId="categoriaId"
      @update:nombreInput="nombreInput = $event"
      @update:mostrarPista="mostrarPista = $event"
      @update:mostrarCategoria="mostrarCategoria = $event"
      @update:categoriaId="categoriaId = $event"
      @agregar="agregarJugador"
      @eliminar="eliminar"
      @editar="editar"
      @guardarEdicion="guardarEdicion"
      @cancelarEdicion="cancelarEdicion"
      @empezar="empezar"
    />

    <RolesFase
      v-if="fase === 'roles'"
      :datosPartida="datosPartida"
      :indiceActual="indiceActual"
      :jugadorActual="jugadorActual"
      :viendoRol="viendoRol"
      :mostrarPista="mostrarPista"
      :mostrarCategoria="mostrarCategoria"
      @verRol="verRol"
      @siguiente="siguienteJugador"
    />

    <JuegoFase
      v-if="fase === 'juego'"
      :datosPartida="datosPartida"
      @votar="fase = 'votacion'"
    />

    <VotacionFase
      v-if="fase === 'votacion'"
      :jugadores="datosPartida.jugadores"
      @votar="votar"
    />

    <FalloFase
      v-if="fase === 'fallo'"
      :votado="votado"
      @volverVotar="fase = 'votacion'"
    />

    <FinFase
      v-if="fase === 'fin'"
      :votado="votado"
      :datosPartida="datosPartida"
      @reiniciar="reiniciar"
    />
  </div>
</template>
