<script>
/**
 * componente para configurar la partida del juego del impostor
 *
 * este componente recibe una serie de atributos que definen la configuracion actual
 * como los jugadores, si esta en modo premium, numero de impostores, etc
 */
import { listaCategorias } from '../data/palabras.js';

export default {
  // estos son los atributos que recibe el componente
  props: [
    'jugadores',
    'editIndice',
    'nombreInput',
    'mostrarPista',
    'mostrarCategoria',
    'categoriaId',
    'numImpostores',
    'premium',
  ],
  data() {
    return {
      categorias: listaCategorias(),
      mensajeError: '',
    };
  },
  computed: {
    /**
     * decide si mostrar la opcion de elegir numero de impostores
     * solo se activa si hay 6 o mas jugadores
     */
    mostrarOpcionNumImpostores() {
      return this.jugadores.length >= 6;
    },
  },
  methods: {
    /**
     * actualiza el valor del input de nombre desde el padre
     */
    updateNombreInput(valor) {
      this.$emit('update:nombreInput', valor);
    },

    /**
     * actualiza la opcion de mostrar pista al impostor (solo si es premium)
     */
    updateMostrarPista(valor) {
      if (this.premium) {
        this.$emit('update:mostrarPista', valor);
      }
    },

    /**
     * actualiza la opcion de mostrar categoria al impostor (solo si es premium)
     */
    updateMostrarCategoria(valor) {
      if (this.premium) {
        this.$emit('update:mostrarCategoria', valor);
      }
    },

    /**
     * actualiza la categoria elegida (solo si es premium)
     */
    updateCategoriaId(valor) {
      if (this.premium) {
        this.$emit('update:categoriaId', valor);
      }
    },

    /**
     * actualiza el numero de impostores elegidos (solo si es premium)
     */
    updateNumImpostores(valor) {
      if (this.premium) {
        this.$emit('update:numImpostores', Number(valor));
      }
    },

    /**
     * añade un nuevo jugador si no existe ya
     */
    agregarJugador() {
      if (this.jugadores.includes(this.nombreInput?.trim())) {
        this.mensajeError = 'No se pueden poner dos nombres iguales.';
        return;
      }
      this.mensajeError = '';
      this.$emit('agregar');
    },

    /**
     * elimina un jugador por indice
     */
    eliminarJugador(i) {
      this.$emit('eliminar', i);
    },

    /**
     * indica que se quiere editar un jugador
     */
    editarJugador(i) {
      this.$emit('editar', i);
    },

    /**
     * guarda la edicion de un jugador
     * comprueba que no haya duplicados
     */
    guardarEdicion() {
      const nombreNuevo = this.nombreInput?.trim();
      if (
        this.jugadores.some(
          (j, i) => i !== this.editIndice && j === nombreNuevo
        )
      ) {
        this.mensajeError = 'No se pueden poner dos nombres iguales.';
        return;
      }
      this.mensajeError = '';
      this.$emit('guardarEdicion');
    },

    /**
     * cancela la edicion actual
     */
    cancelarEdicion() {
      this.$emit('cancelarEdicion');
    },

    /**
     * empieza la partida
     */
    empezarPartida() {
      this.$emit('empezar');
    },

    /**
     * activa/desactiva el modo premium
     */
    togglePremium() {
      this.$emit('cambiarPremium');
    },
  },
};
</script>

<template>
  <div class="seccion">
    <h2>Configurar Partida</h2>

    <div class="grupoInput">
      <!-- Mostrar errores si los hay -->
      <div v-if="mensajeError" class="mensajeError">
        {{ mensajeError }}
      </div>
      <input
        :value="nombreInput"
        @input="updateNombreInput($event.target.value)"
        :placeholder="
          editIndice !== null ? 'Editar nombre' : 'Nombre del jugador'
        "
        @keyup.enter="editIndice !== null ? guardarEdicion() : agregarJugador()"
      />
      <template v-if="editIndice !== null">
        <button
          class="boton"
          @click="guardarEdicion()"
          :disabled="!nombreInput?.trim()"
        >
          Guardar
        </button>
        <button class="boton botonCancelar" @click="cancelarEdicion()">
          Cancelar
        </button>
      </template>
      <button
        v-else
        class="boton"
        @click="agregarJugador()"
        :disabled="!nombreInput?.trim()"
      >
        Agregar
      </button>
    </div>

    <div v-if="jugadores.length" class="listaJugadores">
      <h3>Jugadores ({{ jugadores.length }})</h3>
      <div v-for="(j, i) in jugadores" :key="i" class="jugadorItem">
        <span>{{ j }}</span>
        <div class="botonesAccion">
          <button
            class="boton botonPequeno botonJugador"
            @click="editarJugador(i)"
          >
            Editar
          </button>
          <button
            class="boton botonPequeno botonEliminar"
            @click="eliminarJugador(i)"
          >
            Eliminar
          </button>
        </div>
      </div>
    </div>

    <div class="panelOpciones">
      <h3>Opciones</h3>
      <div class="opcion">
        <label class="switch">
          <input type="checkbox" :checked="premium" @change="togglePremium" />
          <span class="slider"></span>
        </label>
        <span class="label-text">Modo Premium</span>
      </div>

      <template v-if="premium">
        <div class="opcion">
          <div class="grupoToggle">
            <label class="switch-label">
              Mostrar pista
              <label class="switch">
                <input
                  type="checkbox"
                  :checked="mostrarPista"
                  @change="updateMostrarPista($event.target.checked)"
                />
                <span class="slider"></span>
              </label>
            </label>
            <label class="switch-label">
              Mostrar categoria
              <label class="switch">
                <input
                  type="checkbox"
                  :checked="mostrarCategoria"
                  @change="updateMostrarCategoria($event.target.checked)"
                />
                <span class="slider"></span>
              </label>
            </label>
          </div>
        </div>

        <div class="opcion">
          <label for="cat">Categoria:</label>
          <select
            id="cat"
            :value="categoriaId"
            @change="updateCategoriaId($event.target.value)"
          >
            <option value="">Todas (aleatorio)</option>
            <option v-for="c in categorias" :key="c.id" :value="c.id">
              {{ c.nombre }}
            </option>
          </select>
        </div>

        <div v-if="mostrarOpcionNumImpostores" class="opcion">
          <label for="numImpostores">Número de impostores:</label>
          <select
            id="numImpostores"
            :value="numImpostores"
            @change="updateNumImpostores($event.target.value)"
          >
            <option :value="1">1 impostor</option>
            <option :value="2">2 impostores</option>
          </select>
        </div>
      </template>

      <p v-else class="mensajePremium">
        Activa el modo premium para desbloquear más opciones
      </p>
    </div>

    <button
      class="boton botonEmpezar botonGrande botonAncho"
      @click="empezarPartida()"
      :disabled="jugadores.length < 3"
    >
      Empezar (min. 3 jugadores)
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
}

.seccion h2 {
  color: #4a6fa5;
  margin-bottom: 1rem;
  font-size: 1.5rem;
}

.grupoInput {
  display: flex;
  gap: 0.5rem;
  margin-bottom: 1rem;
  flex-wrap: wrap;
}

input,
select {
  flex: 1;
  min-width: 140px;
  padding: 0.75rem;
  font-size: 1rem;
  border: 2px solid #e0e0e0;
  border-radius: 8px;
  transition: border-color 0.2s;
  background: #f9f9f9;
}

input:focus,
select:focus {
  outline: none;
  border-color: #4a6fa5;
}

.boton {
  padding: 0.6rem 1.2rem;
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

.boton:disabled {
  background: #cccccc;
  cursor: not-allowed;
}

.botonPequeno {
  padding: 0.4rem 0.75rem;
  font-size: 0.875rem;
}

.botonGrande {
  padding: 0.9rem 1.5rem;
  font-size: 1.1rem;
}

.botonAncho {
  width: 100%;
  margin-top: 1rem;
}

.botonEmpezar {
  background: #5cb85c;
}

.botonEliminar {
  background: #d9534f;
}

.botonJugador {
  background: #5bc0de;
}

.botonCancelar {
  background: #f0ad4e;
}

.listaJugadores {
  margin: 1rem 0;
  text-align: left;
}

.listaJugadores h3 {
  margin-bottom: 0.5rem;
  font-size: 1rem;
  color: #4a6fa5;
}

.jugadorItem {
  display: flex;
  justify-content: space-between;
  align-items: center;
  padding: 0.75rem;
  background: #f5f5f5;
  border-radius: 8px;
  margin-bottom: 0.5rem;
}

.botonesAccion {
  display: flex;
  gap: 0.25rem;
}

.panelOpciones {
  margin: 1rem 0;
  padding: 1rem;
  background: #f9f9f9;
  border-radius: 12px;
  border: 1px solid #e0e0e0;
}

.panelOpciones h3 {
  margin-bottom: 0.75rem;
  color: #4a6fa5;
  font-size: 1.1rem;
}

.opcion {
  margin-bottom: 0.75rem;
}

.opcion label {
  display: block;
  margin-bottom: 0.4rem;
  font-weight: 500;
  font-size: 0.9rem;
  color: #333;
}

.grupoToggle {
  display: flex;
  flex-direction: column;
  gap: 0.5rem;
}

.switch-label {
  display: flex;
  justify-content: space-between;
  align-items: center;
  font-size: 0.9rem;
  color: #333;
}

.switch {
  position: relative;
  display: inline-block;
  width: 50px;
  height: 24px;
}

.switch input {
  opacity: 0;
  width: 0;
  height: 0;
}

.slider {
  position: absolute;
  cursor: pointer;
  top: 0;
  left: 0;
  right: 0;
  bottom: 0;
  background-color: #ccc;
  transition: 0.4s;
  border-radius: 24px;
}

.slider:before {
  position: absolute;
  content: '';
  height: 18px;
  width: 18px;
  left: 3px;
  bottom: 3px;
  background-color: white;
  transition: 0.4s;
  border-radius: 50%;
}

input:checked + .slider {
  background-color: #4a6fa5;
}

input:checked + .slider:before {
  transform: translateX(26px);
}

.label-text {
  margin-left: 0.5rem;
  vertical-align: middle;
}

.mensajePremium {
  margin-top: 1rem;
  font-size: 0.9rem;
  color: #757575;
  text-align: center;
}

.mensajeError {
  color: #d9534f;
  margin-bottom: 8px;
  font-size: 0.9rem;
}
</style>
