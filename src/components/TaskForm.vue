<script lang="ts" setup>
import { ref } from "vue";

const emit = defineEmits<{ addTask: [newTask: string] }>();

const newTask = ref("");
const error = ref("");

function fromSubmitted() {
  if (!newTask.value.trim()) {
    error.value = "Task cannot be emtpy";
    return;
  }
  emit("addTask", newTask.value.trim());
  newTask.value = "";
  error.value = "";
}
</script>

<template>
  <form @submit.prevent="fromSubmitted">
    <label>
      New Tasks
      <input
        v-model="newTask"
        name="newTask"
        :aria-invalid="!!error || undefined"
        @input="error = ''"
      />
      <small v-if="error" id="invalid-helper">{{ error }}</small>
    </label>
    <div class="button-container">
      <button>Add</button>
    </div>
  </form>
</template>
