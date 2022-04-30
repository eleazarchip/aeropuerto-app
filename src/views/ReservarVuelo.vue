<template>
    <div class="cart-header">
        <div class="is-flex is-justify-content-space-between my-2" style="border-bottom: 1px solid #e8e8e8;">
            <h1 class="title is-3">Reservar asientos en vuelo </h1>    
            <div class="header-actions">
                <div class="buttons">
                    <router-link :to="`/vuelos`" class="button">
                        <span> Regresar </span>
                    </router-link>
                </div>
            </div>
        </div>

    </div>
    <div v-if="loading">
        <span> Cargando la información de los asientos </span>
    </div>
    <div v-else>
        <div class="columns" v-for="(asiento, index) in asientos" :key="index">
            <div class="column">
                {{asiento.numero}}<br/>
                <small class="tag is-link is-light"> {{asiento.clase}} </small>
            </div>
            <div class="column">
                {{asiento.numero}}B<br/>
                <small class="tag is-link is-light"> {{asiento.clase}} </small>
            </div>
            <div class="column">
                {{asiento.numero}}C<br/>
                <small class="tag is-link is-light"> {{asiento.clase}} </small>
            </div>
            <div class="column">
                {{asiento.numero}}D<br/>
                <small class="tag is-link is-light"> {{asiento.clase}} </small>
            </div>
        </div>
        
    </div>
</template>
<script>
import { ref } from '@vue/reactivity';
import { useRoute, onBeforeRouteLeave } from 'vue-router';
import { onMounted } from '@vue/runtime-core';

import axios from 'axios';
import {url_api} from '@/composables/useConstantes.js';

export default {
    name: 'reservar',
    setup() {
        const route = useRoute();

        console.log(route.params.id);

        const asientos = ref([]);
        const loading = ref(true);
        const error = ref(false);
        onMounted(async () => {
            loading.value = true;
            await axios.post(`${url_api}task=Asientos&vuelo=${route.params.id}`)
                .then(res => {
                    console.info(res);
                    asientos.value = res.data.asientos;
                    loading.value = false;
                });
        });

        // onBeforeRouteLeave(() => {
        //     const answer = window.confirm('¿Está seguro que desea salir?')
        //     if( !answer ) return false // false, bloquea la salida
        // });
        return {
            asientos,
            loading
        };
    }
}
</script>
<style>
    
</style>