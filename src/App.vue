<script setup>
import { reactive, ref, watch, onMounted } from 'vue'
import { uid } from 'uid'
import Header from './components/Header.vue'
import Formulario from './components/Formulario.vue'
import Paciente from './components/Paciente.vue'

const pacientes = ref([])
const paciente = reactive({
  id: null,
  mascota: '',
  propietario: '',
  email: '',
  alta: '',
  sintomas: ''
})

watch(
  pacientes,
  (pacientesWatch) => {
    localStorage.setItem('pacientes', JSON.stringify(pacientesWatch))
  },
  { deep: true }
)
onMounted(() => {
  const pacientesLocalStorage = JSON.parse(localStorage.getItem('pacientes'))
  if (pacientesLocalStorage) pacientes.value = pacientesLocalStorage
})

const guardarPaciente = () => {
  if (paciente.id) {
    const index = pacientes.value.findIndex((paciente) => paciente.id === paciente.id)
    pacientes.value[index] = { ...paciente }
  } else {
    pacientes.value.push({ ...paciente, id: uid() })
  }

  Object.assign(paciente, {
    mascota: '',
    propietario: '',
    email: '',
    alta: '',
    sintomas: '',
    id: null
  })
}

const eliminarPaciente = (id) => {
  if (confirm('¿Estás seguro de eliminar este paciente?')) {
    pacientes.value = pacientes.value.filter((paciente) => paciente.id !== id)
    Object.assign(paciente, {
    mascota: '',
    propietario: '',
    email: '',
    alta: '',
    sintomas: '',
    id: null
  })
  }
}

const actualizarPaciente = (id) => {
  const pacienteEditar = pacientes.value.find((paciente) => paciente.id === id)
  Object.assign(paciente, pacienteEditar)
}
</script>

<template>
  <div class="container mx-auto mt-20">
    <Header />

    <div class="mt-12 md:flex gap-5 p-4">
      <Formulario
        v-model:mascota="paciente.mascota"
        v-model:propietario="paciente.propietario"
        v-model:email="paciente.email"
        v-model:alta="paciente.alta"
        v-model:sintomas="paciente.sintomas"
        :id="paciente.id"
        @guardar-paciente="guardarPaciente"
      />

      <div class="md:w-1/2 md:h-screen overflow-y-auto">
        <h2 class="font-black text-3xl text-center">Tus Pacientes</h2>
        <p class="text-lg mt-5 text-center mb-10">
          Tienes <span class="text-indigo-600 font-bold">{{ pacientes.length }}</span> Pacientes
        </p>

        <div v-if="pacientes.length > 0">
          <Paciente
            v-for="paciente in pacientes"
            :paciente="paciente"
            :key="paciente.id"
            @eliminar-paciente="eliminarPaciente"
            @actualizar-paciente="actualizarPaciente"
          />
        </div>

        <p v-else class="mt-20 text-2xl text-center">No hay pacientes</p>
      </div>
    </div>
  </div>
</template>
