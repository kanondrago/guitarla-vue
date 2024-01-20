<script setup>
  import { ref, onMounted } from "vue";
  import { db } from './data/guitarras'
  import Guitarra from './components/Guitarra.vue'
  import Header from './components/Header.vue'
  import Footer from './components/Footer.vue'

  // ARREGLO DE LA BASE DE DATOS DE TODAS LAS GUITARRAS
  const guitarras = ref([])

  // ARREGLO DE LOS PRODUCTOS SELECCIONADOS PARA EL CARRITO DE COMPRAS
  const carrito = ref([])

  // CONTADOR DE LOS PRODUCTOS EN EL CARRITO
  const cantidad = ref(0)


  // CICLO DE VIDA AL INICIAR LA APP - ACA UTILIZAMOS EL FETCH PARA CONSULTAR UNA API
  onMounted(() => {
    console.log('al recargar en el ciclo de vida onMounted')
    guitarras.value = db
  })

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
      
  // ELIMINA UN ITEM DEL CARRITO
  const eliminarProducto = (id) => {
    const indexCarrito = carrito.value.findIndex(producto => producto.id === id)
    carrito.value.splice(indexCarrito,1)
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


</script>

<template>

  <Header
      :carrito="carrito"
      @decrementar-cantidad="decrementarCantidad"
      @incrementar-cantidad="incrementarCantidad"
      @eliminar-producto="eliminarProducto"
      :cantidad="cantidad"
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

