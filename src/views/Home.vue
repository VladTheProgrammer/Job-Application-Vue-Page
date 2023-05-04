<template>
  <!--Navigation Drawer component-->
  <v-navigation-drawer app width="229">
    <v-sheet height="100%" class="gradient"></v-sheet>
  </v-navigation-drawer>
  <HelloWorld />
</template>

<script setup>
import HelloWorld from '@/components/HelloWorld.vue'
</script>

<style scoped>
.gradient {
  background-image: linear-gradient(to bottom, rgb(3, 4, 5), rgb(28, 39, 52));
}
</style>