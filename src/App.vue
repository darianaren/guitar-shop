<script setup>
  import { ref, onMounted } from 'vue'
  import { db } from './data/guitars'
  import Guitar from './components/Guitar.vue'
  import Header from './components/Header.vue'
  import Footer from './components/Footer.vue'

  const guitars = ref([])
  const cart = ref([])

  onMounted(() => {
    guitars.value = db
  })

  const addItem = (guitar) => {
    const itemExists = cart.value.find((item) => item.id === guitar.id)
    if (itemExists) {
      itemExists.amount++
      return
    }
    guitar.amount = 1
    cart.value.push(guitar)
  }

  const removeItem = (guitar) => {
    const itemExists = cart.value.find((item) => item.id === guitar.id)
    if (itemExists) {
      itemExists.amount--
      if (itemExists.amount === 0) {
        cart.value.splice(cart.value.indexOf(itemExists), 1)
      }
      return
    }
  }

  const deleteItem = (guitar) => {
    guitar.amount = 0
    cart.value.splice(cart.value.indexOf(guitar), 1)
  }

  const clearCart = () => {
    cart.value.forEach((item) => {
      item.amount = 0
    })
    cart.value.splice(0, cart.value.length)
  }
</script>

<template>
  <Header
    :cart="cart"
    @delete-item="deleteItem"
    @remove-item="removeItem"
    @add-item="addItem"
    @clear-cart="clearCart"
  />
  <main class="container-xl mt-5">
    <h2 class="text-center">Nuestra Colección</h2>

    <div class="row mt-5">
      <Guitar v-for="guitar in guitars" :guitar="guitar" @add-item="addItem" />
    </div>
  </main>

  <Footer></Footer>
</template>
