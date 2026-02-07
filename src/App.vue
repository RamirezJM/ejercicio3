<script setup>
import { reactive } from 'vue'
import { computed } from 'vue'


const form = reactive({
  nombre: '',
  edad: '',
  biografia: '',
  nivel: '',
  intereses: [],
  pais: null,
  tecnologias: []

})

const paises = [
  { code: "CL", name: "Chile" },
  { code: "AR", name: "Argentina" },
  { code: "PE", name: "Perú" },
  { code: "MX", name: "México" }
]

const tecnologiasDisponibles = [
  "Vue",
  "React",
  "Angular",
  "Svelte",
  "Astro"
]

const MAX_BIO = 200
const bioLength = computed(() => form.biografia.length)

const isFormValid = computed(() => {
  return (
    form.nombre.trim() !== "" &&
    form.edad !== null &&
    form.edad >= 0 &&
    form.edad <= 120 &&
    form.intereses.length >= 1
  )
})

const touched = reactive({
  nombre: false,
  edad: false,
  intereses: false
})

const errors = computed(() => ({
  nombre: form.nombre.trim() === "",
  edad: form.edad === null || !Number.isInteger(form.edad) || form.edad < 0 || form.edad > 120,
  intereses: form.intereses.length === 0
}))

/* const handleSubmit = () => {
  if (!isFormValid.value) return
  console.log("Formulario válido:", form)
} */
/* const handleSubmit = () => {
  touched.nombre = true
  touched.edad = true
  touched.intereses = true

  if (!isFormValid.value) return

  console.log("Formulario enviado", form)
} */
const handleSubmit = () => {
  touched.nombre = true
  touched.edad = true
  touched.intereses = true

  if (!isFormValid.value) return

  alert("Formulario enviado ✅")

  resetForm()
}

const resetForm = () => {
  form.nombre = ""
  form.edad = null
  form.biografia = ""
  form.nivel = ""
  form.intereses = []
  form.pais = null
  form.tecnologias = []
  touched.nombre = false
  touched.edad = false
  touched.intereses = false
}
</script>

<template>
  <h1>Formulario</h1>
  <section class="formulario">
    <form @submit.prevent="handleSubmit()">
      <div class="form-group">
        <label for="nombre">Nombre:</label>
        <input id="nombre" type="text" v-model.trim='form.nombre' @blur="touched.nombre = true"
          :class="{ error: touched.nombre && errors.nombre }" required>
        <small v-if="touched.nombre && errors.nombre">
          El nombre es obligatorio
        </small>
      </div>
      <div class="form-group">
        <label for="edad">Edad:</label>
        <input id="edad" type="number" v-model.number="form.edad" min="0" max="120" @blur="touched.edad = true"
          :class="{ error: touched.edad && errors.edad }" required>
        <small v-if="touched.edad && errors.edad">
          Tu edad debe ser un número entre 0 y 120
        </small>
      </div>
      <div class="form-group">
        <label for="biografia"> Biografía:</label>
        <textarea id="biografia" v-model.lazy="form.biografia"></textarea>
        <p class="counter">{{ bioLength }} / {{ MAX_BIO }} caracteres</p>
      </div>

      <div class="form-group">
        <p>Nivel de programación</p>
        <ul>
          <li><input type="radio" name="nivel" :value="'Trainee'" v-model="form.nivel">Trainee</li>
          <li><input type="radio" name="nivel" :value="'Junior'" v-model="form.nivel">Junior</li>
          <li><input type="radio" name="nivel" :value="'Semi-senior'" v-model="form.nivel">Semi-senior</li>
          <li><input type="radio" name="nivel" :value="'Senior'" v-model="form.nivel">Senior</li>
        </ul>
      </div>
      <div class="form-group">
        <p>Intereses</p>
        <ul @change="touched.intereses = true" :class="{ error: touched.intereses && errors.intereses }">
          <li><input type="checkbox" name="intereses" :value="'Deportes'" v-model="form.intereses">
            Deportes
          </li>
          <li><input type="checkbox" name="intereses" :value="'Cine'" v-model="form.intereses">
            Cine
          </li>
          <li><input type="checkbox" name="intereses" :value="'Música'" v-model="form.intereses">
            Música
          </li>
          <li><input type="checkbox" name="intereses" :value="'Naturaleza'" v-model="form.intereses">
            Naturaleza
          </li>
          <li><input type="checkbox" name="intereses" :value="'Libros'" v-model="form.intereses">
            Libros
          </li>
        </ul>
        <small v-if="touched.intereses && errors.intereses">
          Debes elegir al menos 1 interés
        </small>
      </div>
      <div class="form-group">
        <label for="pais">País:</label>
        <select id="pais" v-model="form.pais">
          <option disabled :value="null">Selecciona un país</option>
          <option v-for="(pais) in paises" :key="pais.code" :value="pais">
            {{ pais.name }}
          </option>
        </select>
      </div>

      <div class="form-group">
        <label for="tech">Tecnologías:</label>

        <select id="tech" v-model="form.tecnologias" multiple>
          <option v-for="(tech, index) in tecnologiasDisponibles" :key="index" :value="tech">
            {{ tech }}
          </option>
        </select>
      </div>

      <button type="submit" :disabled="!isFormValid">Enviar</button>

    </form>
  </section>

  <section class="datos">
    <h2>Datos obtenidos</h2>
    <p>Nombre: {{ form.nombre }}</p>
    <p>Edad: {{ form.edad }}</p>
    <p>Biografía: {{ form.biografia }}</p>
    <p>Nivel: {{ form.nivel }}</p>
    <p>Intereses: </p>
    <ul>
      <li v-for="(interes, index) in form.intereses" :key="index">{{ interes }}</li>
    </ul>
    <p v-if="form.pais">
      País:
      {{ form.pais.name }} ({{ form.pais.code }})
    </p>
    <p>Tecnologías:</p>
    <ul>
      <li v-for="tech in form.tecnologias" :key="tech">
        {{ tech }}
      </li>
    </ul>

  </section>

</template>

<style>
h1 {
  margin: 2em auto;
  text-align: center;
}

.formulario {
  width: 90%;
  margin: 0 auto;
}

form {
  max-width: 600px;
  margin: 0 auto;
  padding: 1em;
  border: 2px solid #424242;
  box-shadow: 1px 1px 2px black;
  background-color: rgb(248, 247, 245);
  display: flex;
  flex-direction: column;
  align-items: center;
  gap: 1em;
  border-radius: 5px;

}

.form-group {
  width: 90%;
  margin: 0 auto;
  display: grid;
  grid-template-columns: 1fr 1fr;
  gap: 1em;
}

label {
  align-self: flex-start;
}

.counter {
  grid-column: 2/3;
  justify-self: flex-end;
  font-weight: 400;
  font-size: 0.9rem;
}

input,
select,
textarea {
  padding: 0.3em;
  border-radius: 5px;
  border-width: 1.5px;
}

input[type="checkbox"],
input[type="radio"] {
  margin: 0.5em;
}

ul {
  list-style-type: none;
}

button {
  padding: 0.5em 1em;
  border: none;
  border-radius: 5px;
  margin-block: 1em;
  font-size: 1.1rem;
  font-weight: 600;
  text-transform: uppercase;
  transition: all 0.2s ease;  
}

/* Estado activo */
button:not(:disabled) {
  background-color: #42b883;
  color: white;
}

button:not(:disabled):hover {
  background-color: #369f6e;
}

/* Estado deshabilitado */
button:disabled {
  background-color: #ccc;
  color: #666;
  cursor: not-allowed;
  opacity: 0.7;
}

button:active{
  background-color: #0a683c;
  transform: scale(1.1);
}

.error {
  border: 2px solid #e74c3c;
  background-color: #fff5f5;
}

small {
  color: #e74c3c;
}

.datos {
  max-width: 600px;
  margin: 1em auto;
}
</style>
