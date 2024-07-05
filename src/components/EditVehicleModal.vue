<template>
  <div class="modal">
    <div class="modal-content">
      <h2>Edit Vehicle</h2>
      <form @submit.prevent="save">
        <label>
          Name:
          <input v-model="editedVehicle.name" />
        </label>
        <label>
          Model:
          <input v-model="editedVehicle.model" />
        </label>
        <label>
          Year:
          <input v-model="editedVehicle.year" type="number" />
        </label>
        <label>
          Price:
          <input v-model="editedVehicle.price" type="number" />
        </label>
        <div class="modal-buttons">
          <button class="save-button" type="submit">Save</button>
          <button class="cancel-button" type="button" @click="$emit('close')">Cancel</button>
        </div>
      </form>
    </div>
  </div>
</template>

<script lang="ts">
import { defineComponent, PropType, ref, watch } from 'vue';

export default defineComponent({
  props: {
    vehicle: {
      type: Object as PropType<{
        id: number,
        name: string,
        model: string,
        year: number,
        price: number
      }>,
      required: true
    }
  },
  setup(props, { emit }) {
    const editedVehicle = ref({ ...props.vehicle });

    watch(() => props.vehicle, (newVal) => {
      editedVehicle.value = { ...newVal };
    });

    const save = () => {
      emit('save', editedVehicle.value);
    };

    return {
      editedVehicle,
      save
    };
  }
});
</script>

<style scoped>
.modal {
  position: fixed;
  top: 0;
  left: 0;
  right: 0;
  bottom: 0;
  background: rgba(0, 0, 0, 0.5);
  display: flex;
  justify-content: center;
  align-items: center;
  padding: 10px;
}
.modal-content {
  background: white;
  padding: 20px;
  border-radius: 8px;

  width: 300px;
}
.modal-content h2 {
  margin-bottom: 20px;
}
.modal-content form label {
  display: block;
  margin: 10px;
}
.modal-content form input {
  width: calc(100% - 20px);
  padding: 8px;
  margin-top: 10px;
  margin-bottom: 10px;
  border-radius: 5px;
}
.modal-buttons {
  display: flex;
  justify-content: space-between;
}
.save-button {
  background-color: #4CAF50;
  color: white;
  border: none;
  padding: 10px 20px;
  cursor: pointer;
  border-radius: 4px;
}
.cancel-button {
  background-color: #f44336;
  color: white;
  border: none;
  padding: 10px 20px;
  cursor: pointer;
  border-radius: 4px;
}
.save-button:hover, .cancel-button:hover {
  opacity: 0.9;
}
</style>
