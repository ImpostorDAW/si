<script setup>
import { computed } from 'vue';
import { listaCategorias } from '../data/palabras.js';

const props = defineProps([
  'jugadores',
  'editIndex',
  'nombreInput',
  'mostrarPista',
  'mostrarCategoria',
  'categoriaId',
]);
const emit = defineEmits([
  'update:nombreInput',
  'update:mostrarPista',
  'update:mostrarCategoria',
  'update:categoriaId',
  'agregar',
  'eliminar',
  'editar',
  'guardarEdicion',
  'cancelarEdicion',
  'empezar',
]);

const categorias = computed(() => listaCategorias());
</script>

<template>
  <div class="seccion">
    <h2>Configurar Partida</h2>

    <div class="grupoInput">
      <input
        :value="nombreInput"
        @input="emit('update:nombreInput', $event.target.value)"
        :placeholder="
          editIndex !== null ? 'Editar nombre' : 'Nombre del jugador'
        "
        @keyup.enter="
          editIndex !== null ? emit('guardarEdicion') : emit('agregar')
        "
      />
      <template v-if="editIndex !== null">
        <button
          class="boton"
          @click="emit('guardarEdicion')"
          :disabled="!nombreInput?.trim()"
        >
          Guardar
        </button>
        <button class="boton botonCancelar" @click="emit('cancelarEdicion')">
          Cancelar
        </button>
      </template>
      <button
        v-else
        class="boton"
        @click="emit('agregar')"
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
            @click="emit('editar', i)"
          >
            Editar
          </button>
          <button
            class="boton botonPequeno botonEliminar"
            @click="emit('eliminar', i)"
          >
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
              @change="emit('update:mostrarPista', $event.target.checked)"
            />
            Mostrar pista
          </label>
          <label>
            <input
              type="checkbox"
              :checked="mostrarCategoria"
              @change="emit('update:mostrarCategoria', $event.target.checked)"
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
          @change="emit('update:categoriaId', $event.target.value)"
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
      @click="emit('empezar')"
      :disabled="jugadores.length < 3"
    >
      Empezar (min. 3 jugadores)
    </button>
  </div>
</template>
