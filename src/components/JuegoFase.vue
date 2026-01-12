<script setup>
defineProps({
  datosPartida: Object,
});

defineEmits(['votar']);
</script>

<template>
  <div class="seccion">
    <h2>A jugar</h2>
    <p>Describan la palabra por turnos.</p>
    <div class="panelInfo">
      <p><strong>Orden:</strong> {{ datosPartida.orden.join(' - ') }}</p>
      <p>
        <strong>Sentido:</strong>
        {{ datosPartida.horario ? 'Horario' : 'Antihorario' }}
      </p>
      <hr />
      <p>
        <em>Los civiles conocen la palabra, el impostor debe adivinarla.</em>
      </p>
    </div>
    <button
      class="boton botonVotar botonGrande botonAncho"
      @click="$emit('votar')"
    >
      Votar
    </button>
  </div>
</template>
