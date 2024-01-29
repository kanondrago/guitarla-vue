<script setup>
  import { ref, onMounted, watch } from "vue";
  import { db } from './data/guitarras'
  import Guitarra from './components/Guitarra.vue'
  import Header from './components/Header.vue'
  import Footer from './components/Footer.vue'

  // ARREGLO DE LA BASE DE DATOS DE TODAS LAS GUITARRAS
  const guitarras = ref([])

  // ARREGLO DE LOS PRODUCTOS SELECCIONADOS PARA EL CARRITO DE COMPRAS
  const carrito = ref([])

  // GUITARRA
  const guitarra = ref({})

  // CONTADOR DE LOS PRODUCTOS EN EL CARRITO
  const cantidad = ref(0)

  // DEFINIENDO EL WATCH
  watch(carrito,() => {
    guardarLocalStorage()
  }, {
    deep: true
  })


  // CICLO DE VIDA AL INICIAR LA APP - ACA UTILIZAMOS EL FETCH PARA CONSULTAR UNA API
  onMounted(() => {
    guitarras.value = db
    guitarra.value = db[3]
    
    // OBTENEMOS EL CARRITO DEL LOCAL STORAGE
    const carritoStorage = localStorage.getItem('carrito')
    if(carritoStorage) {
      carrito.value = JSON.parse(carritoStorage)
      cantidad.value = carrito.value.reduce((contador, producto) => contador + producto.cantidad, 0)
    }
  })

  // LocalStorage
  const guardarLocalStorage = () => {
    localStorage.setItem('carrito', JSON.stringify(carrito.value))
  }

  // AGREGAR UN ITEM AL CARRITO
  const agregarCarrito = (guitarra) => {

      const existeCarrito = carrito.value.findIndex(producto => producto.id===guitarra.id)

      if(existeCarrito>=0) {
        carrito.value[existeCarrito].cantidad++
      }
      else {
        guitarra.cantidad = 1
        carrito.value.push(guitarra)
      }
      actulizarContador(carrito)
  }
      
  const eliminarProducto = (id) => {
    carrito.value = carrito.value.filter(producto => producto.id != id)
    actulizarContador(carrito)
  }

  // DECREMENTA EN 1 LA CANTIDAD DEL ITEM DEL CARRITO
  const decrementarCantidad = (id) => {
    const indexCarrito = carrito.value.findIndex(producto => producto.id === id)
    if(carrito.value[indexCarrito].cantidad<=1) return 
    carrito.value[indexCarrito].cantidad--
    actulizarContador(carrito)
  }

  // INCREMENTA EN 1 LA CANTIDAD DEL ITEM DEL CARRITO
  const incrementarCantidad = (id) => {
    const indexCarrito = carrito.value.findIndex(producto => producto.id === id)
    if(carrito.value[indexCarrito].cantidad>=5) return 
    carrito.value[indexCarrito].cantidad++ 
    actulizarContador(carrito)
  }


  //ACTUALIZA EL CONTADOR DEL CARRITO
  const actulizarContador = (carrito) => {
        const sumaCantidad = ref(0)
        carrito.value.forEach(producto => sumaCantidad.value += producto.cantidad)
        cantidad.value = sumaCantidad.value
  }

  // VACIAR EL CARRITO DE COMPRAS
  const vaciarCarrito = () => {
    carrito.value = []
    cantidad.value = 0
  }

</script>

<template>

  <Header
    @decrementar-cantidad="decrementarCantidad"
    @incrementar-cantidad="incrementarCantidad"
    @eliminar-producto="eliminarProducto"
    @agregar-carrito="agregarCarrito"
    @vaciar-carrito="vaciarCarrito"
    :cantidad="cantidad"
    :carrito="carrito"
    :guitarra="guitarra"
  >

  </Header>

  <main class="container-xl mt-5">
      <h2 class="text-center">Nuestra Colección</h2>

      <div class="row mt-5">
        <Guitarra 
            v-for="guitarra in guitarras" 
            :key="guitarra.id"
            v-bind:guitarraH="guitarra"
            @method-handler="agregarCarrito"
            >
            
        </Guitarra>
      </div>

  </main>

  <Footer/>

</template>

