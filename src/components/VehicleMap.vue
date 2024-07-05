<template>
  <div class="map-container">
    <l-map :zoom="13" :center="[55.753215, 37.620393]" style="height: 500px; width: 100%;">
      <l-tile-layer
          url="https://{s}.tile.openstreetmap.org/{z}/{x}/{y}.png"
      />
      <l-marker
          v-for="vehicle in vehicles"
          :key="vehicle.id"
          :lat-lng="[vehicle.latitude, vehicle.longitude]"
      >
        <l-popup>{{ vehicle.name }} {{ vehicle.model }}</l-popup>
      </l-marker>
    </l-map>
  </div>
</template>

<script lang="ts">
import { defineComponent, onMounted, ref } from 'vue';
import axios from 'axios';
import { LMap, LTileLayer, LMarker, LPopup } from '@vue-leaflet/vue-leaflet';

export default defineComponent({
  components: { LMap, LTileLayer, LMarker, LPopup },
  setup() {
    const vehicles = ref([]);

    onMounted(async () => {
      const response = await axios.get('https://test.tspb.su/test-task/vehicles');
      vehicles.value = response.data;
    });

    return {
      vehicles
    };
  }
});
</script>

<style scoped>
.map-container {
  margin-top: 20px;
}
</style>
