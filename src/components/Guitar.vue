<script setup>
  const props = defineProps({
    guitar: {
      type: Object,
      required: true,
    },
  })

  const emit = defineEmits(['add-item'])
</script>

<template>
  <div class="col-md-6 col-lg-4 my-4 row align-items-center">
    <div class="col-4">
      <img class="img-fluid" :src="`/img/${guitar.imagen}.jpg`" alt="imagen guitarra" />
    </div>
    <div class="col-8">
      <h3 class="text-black fs-4 fw-bold text-uppercase">{{ guitar.nombre }}</h3>
      <p>{{ guitar.descripcion }}</p>
      <p class="fw-black text-primary fs-3">${{ guitar.precio }}</p>
      <p v-if="guitar.amount">Cantidad: {{ guitar.amount }}</p>
      <button type="button" class="btn btn-dark w-100" @click="emit('add-item', guitar)">
        Agregar al Carrito
      </button>
    </div>
  </div>
</template>
