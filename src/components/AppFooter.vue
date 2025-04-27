<template>
  <v-bottom-navigation
    color="blue"
    grow
  >
    <v-btn to="/">
      <v-icon>mdi-arrow-left</v-icon>
      Voltar
    </v-btn>

    <!-- 
    <v-btn>
      <v-icon>mdi-heart</v-icon>
      Favorites
    </v-btn>

    <v-btn>
      <v-icon>mdi-map-marker</v-icon>
      Nearby
    </v-btn> 
    -->
  </v-bottom-navigation>
</template>

<script setup>
import { ref } from 'vue'

const value = ref(0)
</script>
