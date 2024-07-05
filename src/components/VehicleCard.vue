<template>
  <div class="card">
    <h2>{{ vehicle.name }} {{ vehicle.model }}</h2>
    <p>Year: {{ vehicle.year }}</p>
    <p>Price: ${{ vehicle.price }}</p>
    <p>Color: {{ vehicle.color }}</p>
    <button class="edit-button" @click="editVehicle">Edit</button>
    <button class="delete-button" @click="deleteVehicle">Delete</button>
  </div>
</template>

<script lang="ts">
import { defineComponent, PropType } from 'vue';

export default defineComponent({
  props: {
    vehicle: {
      type: Object as PropType<{
        id: number,
        name: string,
        model: string,
        year: number,
        color: string,
        price: number
      }>,
      required: true
    }
  },
  methods: {
    editVehicle() {
      this.$emit('edit', this.vehicle);
    },
    deleteVehicle() {
      this.$emit('delete', this.vehicle.id);
    }
  }
});
</script>

<style scoped>
.card {
  border: 1px solid #ccc;
  padding: 16px;
  margin: 16px;
  border-radius: 8px;
  background-color: #f9f9f9;
  box-shadow: 0 4px 8px rgba(0, 0, 0, 0.1);
  flex: 1;
  min-width: 300px;
  max-width: 30%;
}
.card h2 {
  color: #333;
}
.card p {
  color: #666;
}
.edit-button {
  background-color: #4CAF50;
  color: white;
  border: none;
  padding: 10px 20px;
  cursor: pointer;
  margin-right: 10px;
  border-radius: 4px;
}
.delete-button {
  background-color: #f44336;
  color: white;
  border: none;
  padding: 10px 20px;
  cursor: pointer;
  border-radius: 4px;
}
.edit-button:hover, .delete-button:hover {
  opacity: 0.9;
}
</style>
