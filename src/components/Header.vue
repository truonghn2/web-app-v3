<template>
  <v-app-bar
    color="#C96B8A"
    elevation="2"
  >
    <v-app-bar-nav-icon class="text-white" @click="drawer = !drawer"/>

    <v-app-bar-title class="app-title" style="color: #ffffff">
      B O B A
    </v-app-bar-title>

  </v-app-bar>

  <!--Nav drawer-->
  <v-navigation-drawer
    v-model="drawer"
    location="left"
    temporary
    color="#C96B8A"
    :width="180"
  >
    <v-list nav density="compact">
      <v-list-item
        title="Home"
        prepend-icon="mdi-home"
        class="drawer-item text-white"
        @click="goHome"
      />      
      <v-list-item
        title="All Drinks"
        prepend-icon="mdi-cup"
        class="drawer-item text-white"
        @click=""
      />
      <v-list-item
        title="Favorites"
        prepend-icon="mdi-heart"
        class="drawer-item text-white"
        @click=""
      />

    </v-list>
  </v-navigation-drawer>
</template>

<script setup>
import { ref } from 'vue'
import { useRouter } from 'vue-router'

const drawer = ref(false)
const router = useRouter()

function goHome() {
  drawer.value = false
  router.push('/')
}
</script>

<style scoped>

.drawer-item:hover {
  background-color: rgba(255, 255, 255, 0.22);
  box-shadow: 0 6px 16px rgba(0, 0, 0, 0.20);
}

.drawer-item:hover :deep(.v-icon),
.drawer-item:hover :deep(.v-list-item-title) {
  opacity: 1;
  font-weight: 700;
}

.app-title {
  position: absolute;
  left: 0;
  right: 0;
  text-align: center;
  pointer-events: none;
  font-size: 30px;
}
</style>