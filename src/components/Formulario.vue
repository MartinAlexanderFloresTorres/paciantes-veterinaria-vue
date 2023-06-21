<script setup>
import { reactive } from 'vue'
import Alerta from './Alerta.vue'

const props = defineProps({
  mascota: {
    type: String,
    default: ''
  },
  propietario: {
    type: String,
    default: ''
  },
  email: {
    type: String,
    default: ''
  },
  alta: {
    type: String,
    default: ''
  },
  sintomas: {
    type: String,
    default: ''
  },
  id: {
    type: String,
    default: null
  }
})

const emits = defineEmits([
  'guardar-paciente',
  'update:mascota',
  'update:propietario',
  'update:email',
  'update:alta',
  'update:sintomas'
])

const alerta = reactive({
  mensaje: '',
  tipo: ''
})

const handleSubmit = (e) => {
  e.preventDefault()
  if (Object.values(props).includes('')) {
    alerta.mensaje = 'Todos los campos son obligatorios'
    alerta.tipo = 'error'
    return
  }

  emits('guardar-paciente')

  alerta.mensaje = `Paciente ${props.id ? 'editado' : 'creado'} correctamente`
  alerta.tipo = 'exito'

  if (alerta.mensaje) {
    setTimeout(() => {
      alerta.mensaje = ''
      alerta.tipo = ''
    }, 3000)
  }
}
</script>

<template>
  <div class="md:w-1/2">
    <h2 class="font-black text-3xl text-center">Seguimiento Pacientes</h2>
    <p class="text-lg mt-5 text-center mb-10">
      Añade Pacientes y <span class="text-indigo-600 font-bold">Administralos</span>
    </p>

    <Alerta v-if="alerta.mensaje" :mensaje="alerta.mensaje" :tipo="alerta.tipo" />

    <form class="bg-white shadow-md rounded-lg py-10 px-5 mb-10" @submit="handleSubmit">
      <div class="mb-5">
        <label for="mascota" class="block text-gray-700 font-bold">Mascota</label>
        <input
          type="text"
          class="border-2 w-full mt-2 p-2 rounded-md placeholder-gray-400 outline-gray-400"
          id="mascota"
          name="mascota"
          :value="mascota"
          @input="$emit('update:mascota', $event.target.value)"
          placeholder="Ej: Bettoben"
        />
      </div>

      <div class="mb-5">
        <label for="propietario" class="block text-gray-700 font-bold">Propietario</label>
        <input
          type="text"
          class="border-2 w-full mt-2 p-2 rounded-md placeholder-gray-400 outline-gray-400"
          id="propietario"
          name="propietario"
          :value="propietario"
          @input="$emit('update:propietario', $event.target.value)"
          placeholder="Ej: Alex"
        />
      </div>

      <div class="mb-5">
        <label for="email" class="block text-gray-700 font-bold">Email</label>
        <input
          type="email"
          class="border-2 w-full mt-2 p-2 rounded-md placeholder-gray-400 outline-gray-400"
          id="email"
          name="email"
          :value="email"
          @input="$emit('update:email', $event.target.value)"
          placeholder="Ej: alex@gmail.com"
        />
      </div>

      <div class="mb-5">
        <label for="alta" class="block text-gray-700 font-bold">Fecha Alta</label>
        <input
          type="date"
          class="border-2 w-full mt-2 p-2 rounded-md placeholder-gray-400 outline-gray-400"
          id="alta"
          name="alta"
          :value="alta"
          @input="$emit('update:alta', $event.target.value)"
        />
      </div>

      <div class="mb-5">
        <label for="sintomas" class="block text-gray-700 font-bold">Sintomas</label>
        <textarea
          type="text"
          class="border-2 w-full mt-2 p-2 rounded-md placeholder-gray-400 outline-gray-400"
          id="sintomas"
          name="sintomas"
          :value="sintomas"
          @input="$emit('update:sintomas', $event.target.value)"
          placeholder="Ej: Mi perro no come"
        />
      </div>

      <button
        type="submit"
        class="bg-indigo-600 hover:bg-indigo-700 text-white font-bold py-2 px-4 rounded-md w-full mt-5 transition-all uppercase"
      >
        {{ id ? 'Editar Paciente' : 'Agregar Paciente' }}
      </button>
    </form>
  </div>
</template>
