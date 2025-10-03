<script setup>
import { RouterView } from 'vue-router';
import HomeView from './view/HomeView.vue';
</script>

<template>
<!-- <RouterView/> -->
 <Suspense>
    <template #default>
        <HomeView/>
    </template>
    <template #fallback>
        <p>Loading...</p>
    </template>
 </Suspense>

</template>

<style scoped></style>
