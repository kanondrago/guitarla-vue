<script setup>

    import { ref, defineProps, defineEmits } from 'vue';


    const props = defineProps({
        guitarraH: {
            type: Object,
            required: true
        }
    })

    defineEmits(['methodHandler'])
    


</script>

<template>
    
    <div class="col-md-6 col-lg-4 my-4 row align-items-center">
        <div class="col-4">   
            <img 
                class="img-fluid" 
                :src="'/img/'+guitarraH.imagen+'.jpg'" 
                :alt="'Guitarra: ' + guitarraH.nombre"
            >
        </div>
        <div class="col-8">
            <h3 class="text-black fs-4 fw-bold text-uppercase">{{guitarraH.nombre}}</h3>
            <p>{{ guitarraH.descripcion }}</p>
            <p class="fw-black text-primary fs-3">${{guitarraH.precio}}</p>
            <button 
                type="button"
                class="btn btn-dark w-100 "
                v-on:click="$emit('methodHandler', guitarraH)"
            >Agregar al Carrito
            </button>
        </div>
    </div><!-- FIN GUITARRA -->

</template>