<script setup>
import { RouterLink } from 'vue-router';
</script>

<template>
   <header>
    <nav class="container">
        <div class="branding">
            <img src="../assets/logo.svg" alt="">
            <h1>Help Wanted</h1>
        </div>
        <ul class="nav-routes">
            <RouterLink to="/">Home</RouterLink>
        </ul>
    </nav>
   </header>
</template>



<style lang="scss" scoped>

</style>