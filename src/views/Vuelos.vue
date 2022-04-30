<template>
    <div class="cart-header">
        <div class="is-flex is-justify-content-space-between my-2" style="border-bottom: 1px solid #e8e8e8;">
            <h1 class="title is-3">Lista de vuelos </h1>    
        </div>
    </div>
    <div v-if="loading">
        <span> Cargando información de vuelos </span>
    </div>
    <div class="card mt-1" v-for="(vuelo, index) in vuelos" :key="index" v-else>
        <div class="card-content">
            <div class="is-justify-content-space-between is-flex">
                <div>
                    <img :src="vuelo.url_image" class="logo_aerolinea"/><br/>
                    <small> VUELO: <strong>{{ vuelo.codigo_avion }}-{{ vuelo.numero_vuelo }}</strong> </small>
                </div>
                <div>
                    <strong> {{ vuelo.hora_salida }} - {{ vuelo.hora_llegada }} </strong><br/>
                    <span class="tag is-success"> ON TIME </span>
                </div>
                <div>
                    <strong> {{ vuelo.duracion_viaje }} </strong><br/>
                    <small> {{ vuelo.ciudad_origen }} - {{ vuelo.ciudad_destino }} </small>
                </div>
                <div>
                    <strong> ${{ formatPrice(vuelo.precio) }} </strong><br/>
                    <small> {{ vuelo.nombre_aerolinea }} </small>
                </div>
                <div>
                    <!-- <router-link class="button is-link" :to="`/reservar/${vuelo.numero_vuelo}`"> Consultar asientos </router-link> -->
                    <button class="button is-info" @click="reservarVuelo(vuelo.numero_vuelo)"> Consultar asientos </button>
                    
                </div>
              </div>
        </div>
        <!-- <footer class="card-footer">
            <a href="#" class="card-footer-item">Save</a>
            <a href="#" class="card-footer-item">Edit</a>
            <a href="#" class="card-footer-item">Delete</a>
        </footer> -->
    </div>
</template>
<script>
import axios from 'axios';
import { ref } from '@vue/reactivity';
import { url_api, formatPrice } from '@/composables/useConstantes.js';
import { onMounted } from '@vue/runtime-core';
import { useRouter } from 'vue-router';


export default {
    name: 'Vuelos',
    components: {
    },
    setup() {
        
        const router = useRouter()

        const vuelos = ref([]);
        const loading = ref(true);
        const error = ref(false);
        onMounted(async () => {
            loading.value = true;
            await axios.post(`${url_api}task=ListaVuelos`)
                .then(res => {
                    console.info(res);
                    vuelos.value = res.data.vuelos;
                });
            loading.value = false;
        });

        const reservarVuelo = (id) => {
            router.push({ name: 'reservar', params: { id: id } });
        };


        // const getVuelos = async () => {
        //     loading.value = true;
        //     error.value = false;
        //     try {
        //         const response = await axios.get(url_api, {
        //             task: 'ListaVuelos'
        //         });
        //         vuelos.value = response.data;
        //     } catch (e) {
        //         error.value = true;
        //     }
        //     loading.value = false;
        // };

        // getVuelos();
        return {
            vuelos,
            loading,
            error,

            formatPrice,
            reservarVuelo
        };
    }

}
</script>
<style>
    img.logo_aerolinea {
        width: 100px;
        height: auto;
    }
</style>