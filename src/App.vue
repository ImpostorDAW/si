<script setup>
import { ref } from 'vue';

const fase = ref('gestion'); // 'gestion', 'roles', 'juego', 'votacion', 'resultado'
const nombresGuardados = ref([]);
const nombreNuevo = ref('');
const editandoIndex = ref(null);
const jugadores = ref([]);
const impostor = ref(null);
const palabraSecreta = ref('');
const pistaImpostor = ref('');
const jugadorViendoRol = ref(null);
const rolesVistos = ref([]);
const indiceJugadorActual = ref(0);
const votado = ref(null);
const acerto = ref(false);
const ordenTurnos = ref([]);
const sentidoHorario = ref(true);

function agregarJugador() {
  if (
    nombreNuevo.value.trim() &&
    !nombresGuardados.value.includes(nombreNuevo.value.trim())
  ) {
    nombresGuardados.value.push(nombreNuevo.value.trim());
    nombreNuevo.value = '';
  }
}

function eliminarJugador(index) {
  nombresGuardados.value.splice(index, 1);
}

function editarJugador(index) {
  editandoIndex.value = index;
  nombreNuevo.value = nombresGuardados.value[index];
}

function guardarEdicion() {
  if (nombreNuevo.value.trim() && editandoIndex.value !== null) {
    nombresGuardados.value[editandoIndex.value] = nombreNuevo.value.trim();
    editandoIndex.value = null;
    nombreNuevo.value = '';
  }
}

function cancelarEdicion() {
  editandoIndex.value = null;
  nombreNuevo.value = '';
}

function empezarPartida() {
  if (nombresGuardados.value.length >= 3) {
    asignarRoles();
    fase.value = 'roles';
  }
}

function asignarRoles() {
  const palabras = [
    // Animales
    'Perro',
    'Gato',
    'Serpiente',
    'Tortuga',
    'Elefante',
    'Delfín',
    'Águila',
    'Araña',
    'Caballo',
    'Tiburón',
    'Mariposa',
    'Canguro',
    'Pulpo',
    'Búho',
    'Loro',
    'Rana',
    'Murciélago',
    'Pingüino',
    'León',
    'Abeja',

    // Objetos
    'Pizza',
    'Libro',
    'Café',
    'Guitarra',
    'Avión',
    'Refrigerador',
    'Zapatilla',
    'Paraguas',
    'Ventana',
    'Bicicleta',
    'Teléfono',
    'Cuchillo',
    'Almohada',
    'Semáforo',
    'Escalera',
    'Reloj',
    'Espejo',
    'Campana',
    'Mochila',
    'Vela',
    'Tijeras',
    'Llave',
    'Silla',
    'Lámpara',
    'Martillo',
    'Cámara',
    'Globo',
    'Corona',
    'Dado',
    'Ancla',
    'Brújula',
    'Telescopio',
    'Micrófono',

    // Naturaleza
    'Playa',
    'Montaña',
    'Luna',
    'Árbol',
    'Mar',
    'Nube',
    'Volcán',
    'Río',
    'Cascada',
    'Desierto',
    'Bosque',
    'Cueva',
    'Isla',
    'Arcoíris',
    'Sol',
    'Flor',
    'Cactus',
    'Rayo',
    'Iceberg',
    'Pantano',

    // Comida
    'Hamburguesa',
    'Helado',
    'Sushi',
    'Taco',
    'Pasta',
    'Chocolate',
    'Sandía',
    'Huevo',
    'Queso',
    'Pan',
    'Miel',
    'Limón',
    'Fresa',
    'Banana',
    'Manzana',

    // Lugares
    'Hospital',
    'Cine',
    'Biblioteca',
    'Estadio',
    'Aeropuerto',
    'Iglesia',
    'Prisión',
    'Faro',
    'Castillo',
    'Pirámide',
    'Museo',
    'Cementerio',
    'Circo',

    // Personas/Profesiones
    'Payaso',
    'Astronauta',
    'Pirata',
    'Bombero',
    'Chef',
    'Detective',
    'Ninja',
    'Mago',
    'Vampiro',
    'Fantasma',
    'Sirena',
    'Robot',
    'Momia',
    'Hada',

    // Otros
    'Tatuaje',
    'Dinosaurio',
    'Cohete',
    'Submarino',
    'Paracaídas',
    'Laberinto',
    'Trampolín',
    'Columpio',
    'Acuario',
    'Termómetro',
    'Imán',
    'Piano',
    'Dominó',
    'Ajedrez',
    'Lotería',
    'Carnaval',
    'Boda',
    'Funeral',
  ];
  palabraSecreta.value = palabras[Math.floor(Math.random() * palabras.length)];
  pistaImpostor.value = obtenerPista(palabraSecreta.value);

  const indexImpostor = Math.floor(
    Math.random() * nombresGuardados.value.length
  );
  jugadores.value = nombresGuardados.value.map((nombre, index) => ({
    nombre,
    esImpostor: index === indexImpostor,
    vioRol: false,
  }));
  impostor.value = jugadores.value[indexImpostor];
  rolesVistos.value = [];

  // Determinar orden aleatorio de turnos
  ordenTurnos.value = [...jugadores.value];
  for (let i = ordenTurnos.value.length - 1; i > 0; i--) {
    const j = Math.floor(Math.random() * (i + 1));
    [ordenTurnos.value[i], ordenTurnos.value[j]] = [
      ordenTurnos.value[j],
      ordenTurnos.value[i],
    ];
  }

  // Determinar sentido aleatorio
  sentidoHorario.value = Math.random() < 0.5;
}

function obtenerPista(palabra) {
  const pistas = {
    // Animales - conexiones lejanas
    Perro: 'Cartero', // los perros ladran al cartero
    Gato: 'Egipto', // adoraban a los gatos
    Serpiente: 'Manzana', // Adán y Eva
    Tortuga: 'Liebre', // la fábula
    Elefante: 'Circo', // elefantes de circo
    Delfín: 'Flipper', // la serie
    Águila: 'Billete', // aparece en billetes
    Araña: 'Peter', // Spider-Man
    Caballo: 'Troya', // caballo de Troya
    Tiburón: 'Spielberg', // película Jaws
    Mariposa: 'Efecto', // efecto mariposa
    Canguro: 'Boxeo', // boxean con las patas
    Pulpo: 'Paul', // el pulpo que predecía
    Búho: 'Duolingo', // el búho de la app
    Loro: 'Hombro', // piratas con loro al hombro
    Rana: 'Beso', // besar al sapo
    Murciélago: 'Wayne', // Batman
    Pingüino: 'Esmoquin', // parecen vestidos de gala
    León: 'Metro', // logo de MGM
    Abeja: 'Maya', // la abeja Maya

    // Objetos - referencias culturales
    Pizza: 'Tortugas', // las Tortugas Ninja
    Libro: 'Gusano', // ratón de biblioteca
    Café: 'Friends', // Central Perk
    Guitarra: 'Hoguera', // tocar en fogatas
    Avión: 'Hermanos', // Wright
    Refrigerador: 'Imanes', // decorar la puerta
    Zapatilla: 'Cenicienta', // el zapato de cristal
    Paraguas: 'Poppins', // Mary Poppins
    Ventana: 'Romeo', // escena del balcón
    Bicicleta: 'ET', // escena icónica volando
    Teléfono: 'Casa', // "ET teléfono mi casa"
    Cuchillo: 'Ducha', // Psicosis
    Almohada: 'Guerra', // pelea de almohadas
    Semáforo: 'Reggae', // colores rastafari
    Escalera: 'Serpientes', // el juego de mesa
    Reloj: 'Conejo', // Alicia en el País
    Espejo: 'Reina', // Blancanieves
    Campana: 'Jorobado', // Notre Dame
    Mochila: 'Dora', // la exploradora
    Vela: 'Cumpleaños', // soplar velas
    Tijeras: 'Piedra', // piedra papel tijera
    Llave: 'Corazón', // llave del corazón
    Silla: 'Musical', // juego de las sillas
    Lámpara: 'Genio', // Aladino
    Martillo: 'Thor', // dios nórdico
    Cámara: 'Alma', // "roba el alma"
    Globo: 'Payaso', // IT, Pennywise
    Corona: 'Virus', // COVID
    Dado: 'Jumanji', // la película
    Ancla: 'Tatuaje', // típico de marineros
    Brújula: 'Jack', // Sparrow
    Telescopio: 'Galileo', // el astrónomo
    Micrófono: 'Karaoke', // cantar

    // Naturaleza - asociaciones indirectas
    Playa: 'Tiburón', // película
    Montaña: 'Heidi', // los Alpes
    Luna: 'Lobo', // aúllan a la luna
    Árbol: 'Diciembre', // árbol de Navidad
    Mar: 'Ulises', // La Odisea
    Nube: 'Oveja', // parecen ovejas
    Volcán: 'Pompeya', // ciudad destruida
    Río: 'Amazonas', // el río/empresa
    Cascada: 'Barril', // bajar en barril
    Desierto: 'Espejismo', // ver cosas
    Bosque: 'Caperucita', // cuento
    Cueva: 'Batman', // Batcueva
    Isla: 'Náufrago', // película
    Arcoíris: 'Oro', // duendes y oro
    Sol: 'Ícaro', // mito griego
    Flor: 'Disculpa', // regalar flores
    Cactus: 'Vaquero', // desierto western
    Rayo: 'Potter', // cicatriz
    Iceberg: 'Titanic', // el accidente
    Pantano: 'Shrek', // vive en uno

    // Comida - conexiones raras
    Hamburguesa: 'Bob', // Bob Esponja
    Helado: 'Cerebro', // dolor al comer rápido
    Sushi: 'Cinta', // cinta transportadora
    Taco: 'Martes', // Taco Tuesday
    Pasta: 'Mamá', // comida de mamá
    Chocolate: 'Fábrica', // Willy Wonka
    Sandía: 'Verano', // fruta de verano
    Huevo: 'Gallina', // ¿qué fue primero?
    Queso: 'Ratón', // les gusta
    Pan: 'Jesús', // cuerpo de Cristo
    Miel: 'Winnie', // Pooh
    Limón: 'Vida', // "si la vida te da limones"
    Fresa: 'Leche', // batido
    Banana: 'Resbalón', // cáscara
    Manzana: 'Doctor', // "una manzana al día..."

    // Lugares - referencias
    Hospital: 'Grey', // Grey's Anatomy
    Cine: 'Palomitas', // las compramos ahí
    Biblioteca: 'Silencio', // hay que callarse
    Estadio: 'Ola', // la ola mexicana
    Aeropuerto: 'Despedida', // escenas de películas
    Iglesia: 'Campanas', // suenan en bodas
    Prisión: 'Naranja', // Orange is the New Black
    Faro: 'Soledad', // trabajo solitario
    Castillo: 'Disney', // logo de Disney
    Pirámide: 'Momia', // Egipto
    Museo: 'Noche', // película
    Cementerio: 'Flores', // llevar flores
    Circo: 'Nariz', // nariz de payaso

    // Personas - conexiones extrañas
    Payaso: 'Alcantarilla', // IT
    Astronauta: 'Bandera', // plantar bandera
    Pirata: 'Pata', // pata de palo
    Bombero: 'Dálmata', // perros de bomberos
    Chef: 'Gorro', // el gorro alto
    Detective: 'Pipa', // Sherlock
    Ninja: 'Pizza', // Tortugas Ninja
    Mago: 'Conejo', // sacar del sombrero
    Vampiro: 'Ajo', // les repele
    Fantasma: 'Sábana', // disfraz típico
    Sirena: 'Peine', // se peinan en rocas
    Robot: 'Lata', // hombre de hojalata
    Momia: 'Papel', // papel higiénico disfraz
    Hada: 'Diente', // ratón/hada de los dientes

    // Otros - muy abstractas
    Tatuaje: 'Arrepentimiento', // tatuajes de ex
    Dinosaurio: 'Petróleo', // se convirtieron en eso
    Cohete: 'Cuenta', // cuenta regresiva
    Submarino: 'Beatles', // Yellow Submarine
    Paracaídas: 'Mochila', // parece mochila
    Laberinto: 'Minotauro', // mito griego
    Trampolín: 'Vecino', // verlo desde arriba
    Columpio: 'Infancia', // recuerdos
    Acuario: 'Buscando', // Buscando a Nemo
    Termómetro: 'Mamá', // cuando estás enfermo
    Imán: 'Nevera', // imanes decorativos
    Piano: 'Dientes', // teclas blancas/negras
    Dominó: 'Caída', // efecto dominó
    Ajedrez: 'Gambito', // la serie
    Lotería: 'Renuncia', // renunciar al trabajo
    Carnaval: 'Río', // Río de Janeiro
    Boda: 'Arroz', // tirar arroz
    Funeral: 'Negro', // vestir de negro
  };
  return pistas[palabra] || 'Objeto';
}

function verRol() {
  jugadorViendoRol.value = jugadores.value[indiceJugadorActual.value];
}

function ocultarRol() {
  if (jugadorViendoRol.value && !jugadorViendoRol.value.vioRol) {
    jugadorViendoRol.value.vioRol = true;
    rolesVistos.value.push(jugadorViendoRol.value.nombre);
  }
  jugadorViendoRol.value = null;
  indiceJugadorActual.value++;

  if (indiceJugadorActual.value >= jugadores.value.length) {
    fase.value = 'juego';
  }
}

function terminarJuego() {
  fase.value = 'votacion';
}

function votar(jugador) {
  votado.value = jugador;
  acerto.value = jugador.esImpostor;
  if (jugador.esImpostor) {
    fase.value = 'resultado';
  } else {
    // Si no acertó, volver a votar después de mostrar resultado temporal
    setTimeout(() => {
      fase.value = 'resultado-temporal';
    }, 100);
  }
}

function continuarVotacion() {
  votado.value = null;
  fase.value = 'votacion';
}

function nuevaPartida() {
  fase.value = 'gestion';
  jugadores.value = [];
  impostor.value = null;
  jugadorViendoRol.value = null;
  rolesVistos.value = [];
  indiceJugadorActual.value = 0;
  votado.value = null;
  acerto.value = false;
  ordenTurnos.value = [];
  sentidoHorario.value = true;
}
</script>

<template>
  <div class="impostor-game">
    <h1>🕵️ Juego del Impostor</h1>

    <!-- Fase 1: Gestión de jugadores -->
    <div v-if="fase === 'gestion'" class="fase">
      <h2>Gestión de Jugadores</h2>

      <div class="input-group">
        <input
          v-model="nombreNuevo"
          type="text"
          :placeholder="
            editandoIndex !== null ? 'Editar nombre' : 'Nombre del jugador'
          "
          @keyup.enter="
            editandoIndex !== null ? guardarEdicion() : agregarJugador()
          "
        />
        <button
          v-if="editandoIndex !== null"
          @click="guardarEdicion()"
          :disabled="!nombreNuevo.trim()"
        >
          Guardar
        </button>
        <button
          v-if="editandoIndex !== null"
          @click="cancelarEdicion()"
          class="btn-cancelar"
        >
          Cancelar
        </button>
        <button
          v-else
          @click="agregarJugador()"
          :disabled="!nombreNuevo.trim()"
        >
          Agregar
        </button>
      </div>

      <div v-if="nombresGuardados.length > 0" class="lista-jugadores">
        <h3>Jugadores ({{ nombresGuardados.length }})</h3>
        <div
          v-for="(nombre, index) in nombresGuardados"
          :key="index"
          class="jugador-item"
        >
          <span>{{ nombre }}</span>
          <div class="acciones">
            <button @click="editarJugador(index)" class="btn-editar">
              Editar
            </button>
            <button @click="eliminarJugador(index)" class="btn-eliminar">
              Eliminar
            </button>
          </div>
        </div>
      </div>

      <button
        @click="empezarPartida"
        :disabled="nombresGuardados.length < 3"
        class="btn-empezar"
      >
        Empezar Partida (mínimo 3 jugadores)
      </button>
    </div>

    <!-- Fase 2: Ver roles -->
    <div v-if="fase === 'roles'" class="fase">
      <div v-if="!jugadorViendoRol">
        <h2>Turno de ver rol</h2>
        <p class="contador-jugadores">
          Jugador {{ indiceJugadorActual + 1 }} de {{ jugadores.length }}
        </p>
        <div class="turno-jugador">
          <p class="texto-grande">Es el turno de:</p>
          <h3 class="nombre-jugador">
            {{ jugadores[indiceJugadorActual]?.nombre }}
          </h3>
          <p class="instruccion">
            Pulsa el botón cuando estés listo para ver tu rol en secreto
          </p>
          <button @click="verRol()" class="btn-ver-rol">Ver mi rol</button>
        </div>
      </div>

      <div v-else class="rol-mostrado">
        <h2>{{ jugadorViendoRol.nombre }}</h2>
        <div v-if="jugadorViendoRol.esImpostor" class="impostor-card">
          <h3>Eres el IMPOSTOR</h3>
          <p class="pista">Pista: {{ pistaImpostor }}</p>
          <p>Tu objetivo: descubre cuál es la palabra sin que te descubran.</p>
        </div>
        <div v-else class="normal-card">
          <h3>Eres CIVIL</h3>
          <p class="palabra">
            Tu palabra es: <strong>{{ palabraSecreta }}</strong>
          </p>
          <p>
            Describe la palabra sin decirla directamente para encontrar al
            impostor.
          </p>
        </div>
        <button @click="ocultarRol" class="btn-grande">
          Entendido, siguiente jugador
        </button>
      </div>
    </div>

    <!-- Fase 3: Juego -->
    <div v-if="fase === 'juego'" class="fase">
      <h2>¡Que empiece el juego!</h2>
      <p>Todos conocen su rol. Ahora deben hablar y descubrir al impostor.</p>
      <div class="info-juego">
        <p>
          <strong>Orden de turnos:</strong>
          {{ ordenTurnos.map(j => j.nombre).join(' → ') }}
        </p>
        <p>
          <strong>Sentido:</strong>
          {{ sentidoHorario ? 'Horario ↻' : 'Antihorario ↺' }}
        </p>
        <p><strong>Empieza:</strong> {{ ordenTurnos[0]?.nombre }}</p>
        <hr style="margin: 1em 0; border-color: #ddd" />
        <p><strong>Palabra secreta:</strong> (Solo la saben los civiles)</p>
        <p>
          <strong>El impostor:</strong> Tiene una pista pero no sabe la palabra
          exacta
        </p>
      </div>
      <button @click="terminarJuego" class="btn-terminar">
        Terminar juego y votar
      </button>
    </div>

    <!-- Fase 4: Votación -->
    <div v-if="fase === 'votacion'" class="fase">
      <h2>¿Quién es el impostor?</h2>
      <p>Vota por el jugador que crees que es el impostor:</p>
      <div class="jugadores-votacion">
        <button
          v-for="jugador in jugadores"
          :key="jugador.nombre"
          @click="votar(jugador)"
          class="btn-votar"
        >
          {{ jugador.nombre }}
        </button>
      </div>
    </div>

    <!-- Fase 5: Resultado temporal (fallo) -->
    <div v-if="fase === 'resultado-temporal'" class="fase">
      <div class="resultado-fallado">
        <h2>No es {{ votado.nombre }} 😔</h2>
        <p class="resultado-texto">
          {{ votado.nombre }} no es el impostor. ¡Sigue intentando!
        </p>
      </div>
      <button @click="continuarVotacion" class="btn-continuar">
        Volver a votar
      </button>
    </div>

    <!-- Fase 6: Resultado final (acertado) -->
    <div v-if="fase === 'resultado'" class="fase">
      <div v-if="acerto" class="resultado-acertado">
        <h2>¡Acertaste! 🎉</h2>
        <p class="resultado-texto">
          <strong>{{ votado.nombre }}</strong> era el impostor
        </p>
      </div>
      <div class="info-final">
        <p><strong>Palabra secreta:</strong> {{ palabraSecreta }}</p>
        <p><strong>Pista del impostor:</strong> {{ pistaImpostor }}</p>
      </div>
      <button @click="nuevaPartida" class="btn-reiniciar">Nueva partida</button>
    </div>
  </div>
</template>

<style scoped>
.impostor-game {
  max-width: 600px;
  margin: 2em auto;
  padding: 2em;
  border-radius: 12px;
  background: linear-gradient(135deg, #667eea 0%, #764ba2 100%);
  box-shadow: 0 4px 15px rgba(0, 0, 0, 0.3);
  color: white;
  min-height: 400px;
}

h1 {
  margin-bottom: 1.5em;
  font-size: 2.5em;
}

.fase {
  background: rgba(255, 255, 255, 0.95);
  padding: 2em;
  border-radius: 10px;
  color: #333;
}

input {
  padding: 0.8em;
  font-size: 1.1em;
  border: 2px solid #667eea;
  border-radius: 6px;
  width: 100%;
  max-width: 300px;
  margin: 1em 0;
}

button {
  padding: 0.8em 1.5em;
  font-size: 1.1em;
  border: none;
  border-radius: 6px;
  background: #667eea;
  color: white;
  cursor: pointer;
  margin: 0.5em;
  transition: all 0.3s;
}

button:hover:not(:disabled) {
  background: #764ba2;
  transform: translateY(-2px);
}

button:disabled {
  background: #ccc;
  cursor: not-allowed;
  transform: none;
}

.jugadores-lista {
  display: flex;
  flex-direction: column;
  gap: 1em;
  margin: 1.5em 0;
}

.jugadores-lista button {
  width: 100%;
  font-size: 1.2em;
  padding: 1em;
}

.jugadores-lista button.visto {
  background: #4caf50;
}

.rol-mostrado {
  text-align: center;
}

.impostor-card {
  background: linear-gradient(135deg, #ff416c 0%, #ff4b2b 100%);
  padding: 2em;
  border-radius: 10px;
  color: white;
  margin: 1em 0;
}

.normal-card {
  background: linear-gradient(135deg, #4caf50 0%, #45a049 100%);
  padding: 2em;
  border-radius: 10px;
  color: white;
  margin: 1em 0;
}

.palabra,
.pista {
  font-size: 1.5em;
  margin: 1em 0;
}

.btn-grande {
  font-size: 1.3em;
  padding: 1em 2em;
  margin-top: 1em;
  background: #333;
}

.btn-reiniciar {
  margin-top: 2em;
  background: #ff4b2b;
  font-size: 1.2em;
  width: 100%;
}

.btn-terminar {
  margin-top: 2em;
  background: #ff9800;
  font-size: 1.2em;
  width: 100%;
}

.btn-terminar:hover {
  background: #f57c00;
}

.info-juego {
  background: white;
  padding: 1.5em;
  border-radius: 8px;
  margin: 1.5em 0;
  text-align: left;
}

.info-juego p {
  margin: 0.5em 0;
}

.jugadores-votacion {
  display: flex;
  flex-direction: column;
  gap: 1em;
  margin: 2em 0;
}

.btn-votar {
  width: 100%;
  font-size: 1.3em;
  padding: 1em;
  background: #2196f3;
}

.btn-votar:hover {
  background: #1976d2;
}

.resultado-acertado {
  background: linear-gradient(135deg, #4caf50 0%, #45a049 100%);
  padding: 2em;
  border-radius: 10px;
  color: white;
  margin-bottom: 1.5em;
}

.resultado-fallado {
  background: linear-gradient(135deg, #f44336 0%, #d32f2f 100%);
  padding: 2em;
  border-radius: 10px;
  color: white;
  margin-bottom: 1.5em;
}

.resultado-texto {
  font-size: 1.3em;
  margin-top: 1em;
}

.info-final {
  background: white;
  padding: 1.5em;
  border-radius: 8px;
  margin-bottom: 1.5em;
}

.info-final p {
  margin: 0.8em 0;
  font-size: 1.1em;
}

.input-group {
  display: flex;
  gap: 0.5em;
  margin-bottom: 1.5em;
  flex-wrap: wrap;
}

.input-group input {
  flex: 1;
  margin: 0;
}

.lista-jugadores {
  margin: 1.5em 0;
  text-align: left;
}

.lista-jugadores h3 {
  margin-bottom: 1em;
}

.jugador-item {
  display: flex;
  justify-content: space-between;
  align-items: center;
  padding: 0.8em;
  background: #f9f9f9;
  border-radius: 6px;
  margin-bottom: 0.5em;
}

.jugador-item span {
  font-size: 1.1em;
  font-weight: 500;
}

.acciones {
  display: flex;
  gap: 0.5em;
}

.btn-editar {
  background: #2196f3;
  padding: 0.5em 1em;
  font-size: 0.9em;
}

.btn-editar:hover {
  background: #1976d2;
}

.btn-eliminar {
  background: #f44336;
  padding: 0.5em 1em;
  font-size: 0.9em;
}

.btn-eliminar:hover {
  background: #d32f2f;
}

.btn-cancelar {
  background: #999;
}

.btn-cancelar:hover {
  background: #777;
}

.btn-empezar {
  width: 100%;
  font-size: 1.3em;
  padding: 1em;
  margin-top: 1em;
  background: #4caf50;
}

.btn-empezar:hover:not(:disabled) {
  background: #45a049;
}

.btn-continuar {
  width: 100%;
  font-size: 1.2em;
  padding: 1em;
  margin-top: 1.5em;
  background: #2196f3;
}

.btn-continuar:hover {
  background: #1976d2;
}

.turno-jugador {
  text-align: center;
  padding: 2em;
  background: #f0f0f0;
  border-radius: 10px;
  margin: 2em 0;
}

.contador-jugadores {
  font-size: 0.9em;
  color: #666;
  margin-bottom: 1em;
}

.texto-grande {
  font-size: 1.2em;
  margin-bottom: 0.5em;
  color: #555;
}

.nombre-jugador {
  font-size: 2.5em;
  color: #667eea;
  margin: 0.5em 0;
}

.instruccion {
  font-size: 1em;
  color: #777;
  margin: 1.5em 0;
}

.btn-ver-rol {
  font-size: 1.4em;
  padding: 1em 2em;
  background: #667eea;
  margin-top: 1em;
}

.btn-ver-rol:hover {
  background: #764ba2;
}
</style>
