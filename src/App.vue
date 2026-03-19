<script setup>
  import { ref, onMounted, watch } from 'vue'
  import { db } from './data/guitars'
  import Guitar from './components/Guitar.vue'
  import Header from './components/Header.vue'
  import Footer from './components/Footer.vue'

  const guitars = ref([])
  const cart = ref([])
  const guitar = ref({})

  onMounted(() => {
    guitars.value = db
    guitar.value = guitars.value[3]
    const cartLocalStorage = localStorage.getItem('cart')
    if (cartLocalStorage) {
      const cartSaved = JSON.parse(cartLocalStorage)
      cartSaved.forEach((item) => {
        const guitarSaved = guitars.value.find((guitar) => guitar.id === item.id)
        if (guitarSaved) {
          guitarSaved.amount = item.amount
          cart.value.push(guitarSaved)
        }
      })
    }
  })

  const saveLocalStorage = () => {
    localStorage.setItem('cart', JSON.stringify(cart.value))
  }

  watch(cart, saveLocalStorage, { deep: true })

  const addItem = (guitar) => {
    if (guitar.stock === 0) return

    const itemExists = cart.value.find((item) => item.id === guitar.id)

    if (itemExists) {
      if (guitar.stock === itemExists.amount) return

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
    :guitar="guitar"
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
