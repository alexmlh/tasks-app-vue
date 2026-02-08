<script lang="ts" setup>
import type { TasksFilter } from "../types";
const props = defineProps<{
  filter: TasksFilter;
  currentFilter: TasksFilter;
}>();

const emits = defineEmits<{
  setFilter: [filter: TasksFilter];
}>();
</script>

<template>
  <button
    class="secondary"
    :class="{ contrast: props.filter == props.currentFilter }"
    @click="$emit('setFilter', props.filter)"
  >
    {{ props.filter }}
  </button>
</template>

<style>
button {
  text-transform: capitalize;
}
</style>
