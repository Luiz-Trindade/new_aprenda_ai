<template>
  <v-app>
    <router-view />
  </v-app>
</template>

<script setup>
  //
</script>

<style>
.fade-in {
    animation: fadeIn 0.6s ease-in-out both;
}

@keyframes fadeIn {
    from {
        opacity: 0;
        transform: translateY(10px); /* opcional: leve subida */
    }
    
    to {
        opacity: 1;
        transform: translateY(0);
    }
}
</style>
