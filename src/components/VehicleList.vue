<template>
  <div class="vehicle-list">
    <div class="sort-container">
      <label for="sort">Sort by:</label>
      <select v-model="sortKey" @change="sortVehicles">
        <option value="year">Year</option>
        <option value="price">Price</option>
      </select>
    </div>
    <div class="cards-container">
      <VehicleCard
          v-for="vehicle in sortedVehicles"
          :key="vehicle.id"
          :vehicle="vehicle"
          @edit="editVehicle"
          @delete="deleteVehicle"
      />
    </div>

    <EditVehicleModal
        v-if="isModalOpen"
        :vehicle="currentVehicle"
        @close="closeModal"
        @save="saveVehicle"
    />
  </div>
</template>

<script lang="ts">
import { defineComponent, ref, computed, onMounted } from 'vue';
import axios from 'axios';
import VehicleCard from './VehicleCard.vue';
import EditVehicleModal from './EditVehicleModal.vue';

export default defineComponent({
  components: { VehicleCard, EditVehicleModal },
  setup() {
    const vehicles = ref([]);
    const sortKey = ref('year');
    const currentVehicle = ref(null);
    const isModalOpen = ref(false);

    onMounted(async () => {
      const response = await axios.get('https://test.tspb.su/test-task/vehicles');
      vehicles.value = response.data;
    });

    const sortedVehicles = computed(() => {
      return [...vehicles.value].sort((a, b) => (a[sortKey.value] > b[sortKey.value] ? 1 : -1));
    });

    const editVehicle = (vehicle: any) => {
      currentVehicle.value = { ...vehicle };
      isModalOpen.value = true;
    };

    const closeModal = () => {
      isModalOpen.value = false;
    };

    const saveVehicle = (updatedVehicle: any) => {
      const index = vehicles.value.findIndex(v => v.id === updatedVehicle.id);
      if (index !== -1) {
        vehicles.value[index] = updatedVehicle;
      }
      isModalOpen.value = false;
    };

    const deleteVehicle = (id: number) => {
      vehicles.value = vehicles.value.filter(vehicle => vehicle.id !== id);
    };

    return {
      vehicles,
      sortKey,
      sortedVehicles,
      editVehicle,
      deleteVehicle,
      currentVehicle,
      isModalOpen,
      closeModal,
      saveVehicle
    };
  }
});
</script>

<style scoped>
.vehicle-list {
  display: flex;
  flex-direction: column;
  align-items: center;
  width: 100%;
}
.sort-container {
  margin-bottom: 20px;
}
.cards-container {
  display: flex;
  flex-wrap: wrap;
  justify-content: space-between;
  width: 100%;
}
</style>
