<script>
import ConfigFase from './components/ConfigFase.vue';
import './components/estilos.css';
import FalloFase from './components/FalloFase.vue';
import FinFase from './components/FinFase.vue';
import JuegoFase from './components/JuegoFase.vue';
import RolesFase from './components/RolesFase.vue';
import VictoriaImpostor from './components/VictoriaImpostor.vue';
import VotacionFase from './components/VotacionFase.vue';
import { palabraAleatoria } from './data/palabras.js';

export default {
  components: {
    ConfigFase,
    FalloFase,
    FinFase,
    JuegoFase,
    RolesFase,
    VotacionFase,
    VictoriaImpostor,
  },
  data() {
    return {
      fase: 'config',
      jugadores: this.cargarJugadores(),
      nombreInput: '',
      editIndex: null,
      mostrarPista: true,
      mostrarCategoria: true,
      categoriaId: '',
      palabra: '',
      orden: [],
      horario: true,
      impostorIndex: null,
      viendoRol: null,
      indiceActual: 0,
      votadoIndex: null,
      votados: [],
    };
  },
  methods: {
    cargarJugadores() {
      const guardados = localStorage.getItem('jugadores');
      return guardados ? JSON.parse(guardados) : [];
    },
    guardarJugadores() {
      localStorage.setItem('jugadores', JSON.stringify(this.jugadores));
    },
    agregarJugador() {
      const nombre = this.nombreInput.trim();
      if (nombre && !this.jugadores.includes(nombre)) {
        this.jugadores.push(nombre);
        this.nombreInput = '';
        this.guardarJugadores();
      }
    },
    eliminar(i) {
      this.jugadores.splice(i, 1);
      this.guardarJugadores();
    },
    editar(i) {
      this.editIndex = i;
      this.nombreInput = this.jugadores[i];
    },
    guardarEdicion() {
      if (this.nombreInput.trim() && this.editIndex !== null) {
        this.jugadores[this.editIndex] = this.nombreInput.trim();
        this.cancelarEdicion();
        this.guardarJugadores();
      }
    },
    cancelarEdicion() {
      this.editIndex = null;
      this.nombreInput = '';
    },
    empezar() {
      if (this.jugadores.length < 3) return;
      this.impostorIndex = Math.floor(Math.random() * this.jugadores.length);
      this.orden = this.jugadores.slice();
      this.orden.sort(() => Math.random() - 0.5);
      this.palabra = palabraAleatoria(this.categoriaId || null);
      this.horario = Math.random() < 0.5;
      this.indiceActual = 0;
      this.viendoRol = null;
      this.votadoIndex = null;
      this.fase = 'roles';
    },
    verRol() {
      this.viendoRol = this.indiceActual;
    },
    siguienteJugador() {
      this.viendoRol = null;
      this.indiceActual++;
      if (this.indiceActual >= this.jugadores.length) {
        this.fase = 'juego';
      }
    },
    votar(i) {
      if (i !== this.impostorIndex && this.votados.includes(i)) return;
      this.votadoIndex = i;
      if (i !== this.impostorIndex) {
        this.votados.push(i);
      }
      if (this.jugadores.length - this.votados.length === 2) {
        this.fase = 'victoriaImpostor';
      } else {
        this.fase = i === this.impostorIndex ? 'fin' : 'fallo';
      }
    },
    reiniciar() {
      this.fase = 'config';
      this.palabra = '';
      this.orden = [];
      this.horario = true;
      this.impostorIndex = null;
      this.viendoRol = null;
      this.indiceActual = 0;
      this.votadoIndex = null;
      this.votados = [];
    },
  },
  watch: {
    jugadores: {
      handler() {
        this.guardarJugadores();
      },
      deep: true,
    },
  },
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
      :jugadores="jugadores"
      :indiceActual="indiceActual"
      :viendoRol="viendoRol"
      :impostorIndex="impostorIndex"
      :palabra="palabra"
      :mostrarPista="mostrarPista"
      :mostrarCategoria="mostrarCategoria"
      @verRol="verRol"
      @siguiente="siguienteJugador"
    />

    <JuegoFase
      v-if="fase === 'juego'"
      :orden="orden"
      :horario="horario"
      :palabra="palabra"
      :impostorIndex="impostorIndex"
      @votar="fase = 'votacion'"
    />

    <VotacionFase
      v-if="fase === 'votacion'"
      :jugadores="jugadores"
      :impostorIndex="impostorIndex"
      :votados="votados"
      @votar="votar"
    />

    <FalloFase
      v-if="fase === 'fallo'"
      :jugadores="jugadores"
      :votadoIndex="votadoIndex"
      @volverVotar="fase = 'votacion'"
    />

    <FinFase
      v-if="fase === 'fin'"
      :jugadores="jugadores"
      :votadoIndex="votadoIndex"
      :palabra="palabra"
      @reiniciar="reiniciar"
    />
    <VictoriaImpostor
      v-if="fase === 'victoriaImpostor'"
      :jugadores="jugadores"
      :impostorIndex="impostorIndex"
      :palabra="palabra"
      @reiniciar="reiniciar"
    />
  </div>
</template>
