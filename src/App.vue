<script>
/**
 * componente principal de la aplicacion del juego del impostor
 * gestiona todas las fases del juego: configuracion, roles, juego, votacion, etc
 * tambien se encarga de gestionar el estado de los jugadores, impostores, temporizador y modo premium
 */

import AcertoFase from './components/AcertoFase.vue';
import ConfigFase from './components/ConfigFase.vue';
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
    AcertoFase,
  },
  data() {
    return {
      fase: 'config',
      jugadores: this.cargarJugadores(),
      nombreInput: '',
      editIndice: null,
      mostrarPista: this.cargarMostrarPista(),
      mostrarCategoria: this.cargarMostrarCategoria(),
      categoriaId: this.cargarCategoriaId(),
      numImpostores: this.cargarNumImpostores(),
      palabra: '',
      orden: [],
      horario: true,
      impostorIndices: [],
      viendoRol: null,
      indiceActual: 0,
      votadoIndice: null,
      votados: [],
      temporizadorActivo: false,
      tiempoRestante: 60,
      temporizadorInterval: null,
      premium: this.cargarPremium(),
    };
  },
  methods: {
    /**
     * carga la lista de jugadores desde localStorage
     */
    cargarJugadores() {
      const guardados = localStorage.getItem('jugadores');
      return guardados ? JSON.parse(guardados) : [];
    },

    /**
     * carga el estado del modo premium desde localStorage
     */
    cargarPremium() {
      const guardado = localStorage.getItem('premium');
      return guardado ? JSON.parse(guardado) : false;
    },

    /**
     * carga el estado de mostrar pista desde localStorage
     */
    cargarMostrarPista() {
      const guardado = localStorage.getItem('mostrarPista');
      return guardado ? JSON.parse(guardado) : true;
    },

    /**
     * carga el estado de mostrar categoria desde localStorage
     */
    cargarMostrarCategoria() {
      const guardado = localStorage.getItem('mostrarCategoria');
      return guardado ? JSON.parse(guardado) : false;
    },

    /**
     * carga la categoria elegida desde localStorage
     */
    cargarCategoriaId() {
      return localStorage.getItem('categoriaId') || '';
    },

    /**
     * carga el numero de impostores desde localStorage
     */
    cargarNumImpostores() {
      const guardado = localStorage.getItem('numImpostores');
      return guardado ? parseInt(guardado) : 1;
    },

    /**
     * guarda la lista de jugadores en localStorage
     */
    guardarJugadores() {
      localStorage.setItem('jugadores', JSON.stringify(this.jugadores));
    },

    /**
     * guarda el estado del modo premium en localStorage
     */
    guardarPremium() {
      localStorage.setItem('premium', JSON.stringify(this.premium));
    },

    /**
     * guarda el numero de impostores en localStorage
     */
    guardarNumImpostores() {
      localStorage.setItem('numImpostores', this.numImpostores.toString());
    },

    /**
     * guarda las opciones avanzadas en localStorage
     */
    guardarOpciones() {
      localStorage.setItem('mostrarPista', this.mostrarPista);
      localStorage.setItem('mostrarCategoria', this.mostrarCategoria);
      localStorage.setItem('categoriaId', this.categoriaId);
    },

    /**
     * alterna el estado del modo premium
     * si se desactiva, se fuerzan los valores por defecto excepto numImpostores y categoriaId
     */
    cambiarPremium() {
      this.premium = !this.premium;
      this.guardarPremium();

      if (!this.premium) {
        this.mostrarPista = true;
        this.mostrarCategoria = false;
        this.categoriaId = '';
        this.numImpostores = 1;
        this.guardarNumImpostores();
      }
      this.guardarOpciones();
    },

    /**
     * agrega un nuevo jugador a la lista
     */
    agregarJugador() {
      const nombre = this.nombreInput.trim();
      if (nombre && !this.jugadores.includes(nombre)) {
        this.jugadores.push(nombre);
        this.nombreInput = '';
        this.guardarJugadores();
      }
    },

    /**
     * elimina un jugador de la lista por su indice
     */
    eliminarJugador(i) {
      this.jugadores.splice(i, 1);
      this.guardarJugadores();
    },

    /**
     * prepara la edicion de un jugador
     */
    editarJugador(i) {
      this.editIndice = i;
      this.nombreInput = this.jugadores[i];
    },

    /**
     * guarda la edicion de un jugador
     */
    guardarEdicion() {
      if (this.nombreInput.trim() && this.editIndice !== null) {
        this.jugadores[this.editIndice] = this.nombreInput.trim();
        this.cancelarEdicion();
        this.guardarJugadores();
      }
    },

    /**
     * cancela la edicion actual
     */
    cancelarEdicion() {
      this.editIndice = null;
      this.nombreInput = '';
    },

    /**
     * inicia la partida: asigna roles, elige palabra, etc
     */
    empezarPartida() {
      if (this.jugadores.length < 3) return;

      let numImpostores = this.premium ? this.numImpostores : 1;

      if (this.jugadores.length < 6) {
        numImpostores = 1;
      }

      this.guardarNumImpostores();

      const indices = Array.from(
        { length: this.jugadores.length },
        (_, i) => i
      );
      const shuffled = indices.sort(() => Math.random() - 0.5);
      this.impostorIndices = shuffled.slice(0, numImpostores);
      this.orden = this.jugadores.slice();
      this.palabra = palabraAleatoria(this.categoriaId || null);
      this.horario = Math.random() < 0.5;
      this.indiceInicio = Math.floor(Math.random() * this.jugadores.length);
      this.indiceActual = 0;
      this.viendoRol = null;
      this.votadoIndice = null;
      this.votados = [];
      this.fase = 'roles';
    },

    /**
     * marca que el jugador actual esta viendo su rol
     */
    verRol() {
      this.viendoRol = this.indiceActual;
    },

    /**
     * pasa al siguiente jugador en la fase de roles (uno detrás de otro)
     */
    siguienteJugador() {
      this.viendoRol = null;
      this.indiceActual++;

      if (this.indiceActual >= this.jugadores.length) {
        this.fase = 'juego';
        this.indiceActual = this.indiceInicio;
      }
    },

    /**
     * gestiona el voto de un jugador
     * si es impostor, se quita de la lista de impostores
     * si ya no quedan impostores vivos, gana el equipo de civiles
     * si no, se pasa a la pantalla de acierto o fallo
     */
    votar(i) {
      // Si el jugador votado es un impostor
      if (this.impostorIndices.includes(i)) {
        // Añadirlo a votados para que no se pueda votar de nuevo
        if (!this.votados.includes(i)) {
          this.votados.push(i);
        }

        // Eliminar al impostor de la lista de impostores vivos
        this.impostorIndices = this.impostorIndices.filter(idx => idx !== i);

        // Si ya no quedan impostores vivos, gana el equipo de civiles
        if (this.impostorIndices.length === 0) {
          this.votadoIndice = i;
          this.detenerTemporizador();
          this.fase = 'fin';
          return;
        }

        // Si aún quedan impostores, mostrar que acertaron
        this.votadoIndice = i;
        this.fase = 'acerto';
        return;
      }

      // Si no es impostor, se añade a votados
      if (!this.votados.includes(i)) {
        this.votados.push(i);
      }

      // Si se vota a un civil, mostrar fallo
      this.votadoIndice = i;
      this.fase = 'fallo';
    },

    /**
     * evalua si algun equipo ha ganado tras un voto
     * si los impostores tienen ventaja, ganan ellos
     */
    evaluarVictorias() {
      const civilesVivos =
        this.jugadores.length -
        this.votados.length -
        this.impostorIndices.length;
      const impostoresRestantes = this.impostorIndices.length;

      if (impostoresRestantes >= civilesVivos) {
        this.detenerTemporizador();
        this.fase = 'victoriaImpostor';
      } else {
        this.fase = 'votacion';
        this.iniciarTemporizador();
      }
    },

    /**
     * inicia el temporizador de la fase de votacion
     */
    iniciarTemporizador() {
      if (this.temporizadorActivo) return;

      this.tiempoRestante = 60;
      this.temporizadorActivo = true;

      this.temporizadorInterval = setInterval(() => {
        this.tiempoRestante--;
        if (this.tiempoRestante <= 0) {
          this.tiempoRestante = 0;
          this.detenerTemporizador();
        }
      }, 1000);
    },

    /**
     * detiene el temporizador actual
     */
    detenerTemporizador() {
      clearInterval(this.temporizadorInterval);
      this.temporizadorActivo = false;
      this.temporizadorInterval = null;
    },

    /**
     * reinicia la partida completa
     */
    reiniciarPartida() {
      this.detenerTemporizador();
      this.fase = 'config';
      this.palabra = '';
      this.orden = [];
      this.horario = true;
      this.impostorIndices = [];
      this.viendoRol = null;
      this.indiceActual = 0;
      this.votadoIndice = null;
      this.votados = [];
      // No reiniciar numImpostores aquí, mantener el valor actual
      this.tiempoRestante = 60;
    },
  },
  watch: {
    jugadores: {
      handler() {
        this.guardarJugadores();
      },
      deep: true,
    },
    numImpostores() {
      this.guardarNumImpostores();
    },
    mostrarPista() {
      localStorage.setItem('mostrarPista', this.mostrarPista);
    },
    mostrarCategoria() {
      localStorage.setItem('mostrarCategoria', this.mostrarCategoria);
    },
    categoriaId() {
      localStorage.setItem('categoriaId', this.categoriaId);
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
      :editIndice="editIndice"
      :nombreInput="nombreInput"
      :mostrarPista="mostrarPista"
      :mostrarCategoria="mostrarCategoria"
      :categoriaId="categoriaId"
      :numImpostores="numImpostores"
      :premium="premium"
      @update:nombreInput="nombreInput = $event"
      @update:mostrarPista="mostrarPista = $event"
      @update:mostrarCategoria="mostrarCategoria = $event"
      @update:categoriaId="categoriaId = $event"
      @update:numImpostores="numImpostores = $event"
      @agregar="agregarJugador"
      @eliminar="eliminarJugador"
      @editar="editarJugador"
      @guardarEdicion="guardarEdicion"
      @cancelarEdicion="cancelarEdicion"
      @empezar="empezarPartida"
      @cambiarPremium="cambiarPremium"
    />

    <RolesFase
      v-if="fase === 'roles'"
      :jugadores="jugadores"
      :indiceActual="indiceActual"
      :viendoRol="viendoRol"
      :impostorIndices="impostorIndices"
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
      :impostorIndices="impostorIndices"
      :indiceInicio="indiceInicio"
      @votar="
        fase = 'votacion';
        iniciarTemporizador();
      "
    />

    <VotacionFase
      v-if="fase === 'votacion'"
      :jugadores="jugadores"
      :impostorIndices="impostorIndices"
      :votados="votados"
      :tiempoRestante="tiempoRestante"
      @votar="votar"
    />

    <FalloFase
      v-if="fase === 'fallo'"
      :jugadores="jugadores"
      :votadoIndice="votadoIndice"
      @volverVotar="evaluarVictorias"
    />

    <AcertoFase
      v-if="fase === 'acerto'"
      :jugadores="jugadores"
      :votadoIndice="votadoIndice"
      @volverVotar="evaluarVictorias"
    />

    <FinFase
      v-if="fase === 'fin'"
      :jugadores="jugadores"
      :votadoIndice="votadoIndice"
      :palabra="palabra"
      @reiniciar="reiniciarPartida"
    />
    <VictoriaImpostor
      v-if="fase === 'victoriaImpostor'"
      :jugadores="jugadores"
      :impostorIndices="impostorIndices"
      :palabra="palabra"
      @reiniciar="reiniciarPartida"
    />
  </div>
</template>

<style scoped>
.juego {
  max-width: 560px;
  margin: 2rem auto;
  padding: 2rem;
  border-radius: 12px;
  background: linear-gradient(135deg, #4a6fa5, #3a5a80);
  box-shadow: 0 8px 32px rgba(0, 0, 0, 0.3);
  color: #fff;
  min-height: 100vh;
}

.juego h1 {
  margin-bottom: 1.5rem;
  font-size: 1.75rem;
  text-align: center;
}

@media (max-width: 480px) {
  .juego {
    margin: 1rem;
    padding: 1rem;
  }
  .juego h1 {
    font-size: 1.4rem;
  }
}
</style>
