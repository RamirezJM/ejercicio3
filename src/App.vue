<script setup>
import { reactive } from 'vue'
import {computed} from 'vue'


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

const handleSubmit = () => {
  if (!isFormValid.value) return
  console.log("Formulario válido:", form)
}
</script>

<template>
  <h1>Formulario</h1>
  <form @submit.prevent="handleSubmit()">
    <label> Nombre:
      <input type="text" v-model.trim='form.nombre' required>
    </label>
    <label> Edad:
      <input type="number" v-model.number="form.edad" min="0" max="120">
    </label>
    <label> Biografía:
      <textarea v-model.lazy="form.biografia"></textarea>
    </label>
    <p>{{ bioLength }} / {{ MAX_BIO }} caracteres</p>
    <p>Nivel de programación</p>
    <ol>
      <li><input type="radio" name="nivel" :value="'Trainee'" v-model="form.nivel">Trainee</li>
      <li><input type="radio" name="nivel" :value="'Junior'" v-model="form.nivel">Junior</li>
      <li><input type="radio" name="nivel" :value="'Semi-senior'" v-model="form.nivel">Semi-senior</li>
      <li><input type="radio" name="nivel" :value="'Senior'" v-model="form.nivel">Senior</li>
    </ol>
    <p>Intereses</p>
    <ul>
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
    <label>
      País:
      <select v-model="form.pais">
        <option disabled :value="null">Selecciona un país</option>
        <option v-for="(pais) in paises" :key="pais.code" :value="pais">
          {{ pais.name }}
        </option>
      </select>
    </label>

    <label>
      Tecnologías:
      <select v-model="form.tecnologias" multiple>
        <option v-for="(tech, index) in tecnologiasDisponibles" :key="index" :value="tech">
          {{ tech }}
        </option>
      </select>
    </label>

    <button type="submit" :disabled="!isFormValid">Enviar</button>

  </form>

  <section class="datos">
    <h2>Datos obtenidos</h2>
    <p>{{ form.nombre }}</p>
    <p>{{ form.edad }}</p>
    <p>{{ form.biografia }}</p>
    <p>Nivel {{ form.nivel }}</p>
    <p>Intereses</p>
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

<style></style>
