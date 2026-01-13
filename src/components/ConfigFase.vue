<script>
import { listaCategorias } from '../data/palabras.js';

export default {
  props: [
    'jugadores',
    'editIndex',
    'nombreInput',
    'mostrarPista',
    'mostrarCategoria',
    'categoriaId',
  ],
  data() {
    return {
      categorias: listaCategorias(),
      mensajeError: '',
    };
  },
  methods: {
    updateNombreInput(valor) {
      this.$emit('update:nombreInput', valor);
    },
    updateMostrarPista(valor) {
      this.$emit('update:mostrarPista', valor);
    },
    updateMostrarCategoria(valor) {
      this.$emit('update:mostrarCategoria', valor);
    },
    updateCategoriaId(valor) {
      this.$emit('update:categoriaId', valor);
    },
    agregar() {
      if (this.jugadores.includes(this.nombreInput?.trim())) {
        this.mensajeError = 'No se pueden poner dos nombres iguales.';
        return;
      }
      this.mensajeError = '';
      this.$emit('agregar');
    },
    eliminar(i) {
      this.$emit('eliminar', i);
    },
    editar(i) {
      this.$emit('editar', i);
    },
    guardarEdicion() {
      const nombreNuevo = this.nombreInput?.trim();
      if (
        this.jugadores.some((j, i) => i !== this.editIndex && j === nombreNuevo)
      ) {
        this.mensajeError = 'No se pueden poner dos nombres iguales.';
        return;
      }
      this.mensajeError = '';
      this.$emit('guardarEdicion');
    },
    cancelarEdicion() {
      this.$emit('cancelarEdicion');
    },
    empezar() {
      this.$emit('empezar');
    },
  },
};
</script>

<template>
  <div class="seccion">
    <h2>Configurar Partida</h2>

    <div class="grupoInput">
      <div
        v-if="mensajeError"
        class="mensajeError"
        style="color: red; margin-bottom: 8px"
      >
        {{ mensajeError }}
      </div>
      <input
        :value="nombreInput"
        @input="updateNombreInput($event.target.value)"
        :placeholder="
          editIndex !== null ? 'Editar nombre' : 'Nombre del jugador'
        "
        @keyup.enter="editIndex !== null ? guardarEdicion() : agregar()"
      />
      <template v-if="editIndex !== null">
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
        @click="agregar()"
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
          <button class="boton botonPequeno botonJugador" @click="editar(i)">
            Editar
          </button>
          <button class="boton botonPequeno botonEliminar" @click="eliminar(i)">
            Eliminar
          </button>
        </div>
      </div>
    </div>

    <div class="panelOpciones">
      <h3>Opciones</h3>
      <div class="opcion">
        <label>Ayuda para el impostor:</label>
        <div class="grupoCheckbox">
          <label>
            <input
              type="checkbox"
              :checked="mostrarPista"
              @change="updateMostrarPista($event.target.checked)"
            />
            Mostrar pista
          </label>
          <label>
            <input
              type="checkbox"
              :checked="mostrarCategoria"
              @change="updateMostrarCategoria($event.target.checked)"
            />
            Mostrar categoria
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
    </div>

    <button
      class="boton botonEmpezar botonGrande botonAncho"
      @click="empezar()"
      :disabled="jugadores.length < 3"
    >
      Empezar (min. 3 jugadores)
    </button>
  </div>
</template>
