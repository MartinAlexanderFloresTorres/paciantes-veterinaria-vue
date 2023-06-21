<script setup>
defineProps({
  mensaje: String,
  tipo: String
})
</script>

<template>
  <div
    class="border-l-4 p-4 mb-5 font-semibold text-center"
    :class="[
      tipo === 'error'
        ? 'border-red-400 text-red-700 bg-red-100'
        : 'border-green-400 text-green-700 bg-green-100'
    ]"
  >
    {{ mensaje }}
  </div>
</template>
