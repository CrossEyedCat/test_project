<template>
  <div id="app">
    <VehicleList />
    <VehicleMap />
  </div>
</template>

<script lang="ts">
import { defineComponent } from 'vue';
import VehicleList from './components/VehicleList.vue';
import VehicleMap from './components/VehicleMap.vue';

export default defineComponent({
  name: 'App',
  components: {
    VehicleList,
    VehicleMap
  }
});
</script>
