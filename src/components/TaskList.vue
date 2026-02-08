<script lang="ts" setup>
import type { Task } from "../types";
const props = defineProps<{ tasks: Task[] }>();
const emits = defineEmits<{
  toggleDone: [id: string];
  removeTask: [id: string];
}>();
</script>

<template>
  <TransitionGroup name="list" tag="div" class="task-list">
    <article v-for="task in tasks" :key="task.id">
      <label>
        <input
          type="checkbox"
          @input="emits('toggleDone', task.id)"
          :checked="task.done"
        />
        <span :class="{ done: task.done }">
          {{ task.title }}
        </span>
      </label>
      <button class="outline" @click="emits('removeTask', task.id)">X</button>
    </article>
  </TransitionGroup>
</template>

<style>
.task-list {
  margin-top: 1rem;
}
article {
  display: flex;
  justify-content: space-between;
  align-items: center;
}
.done {
  text-decoration: line-through;
}

.list-enter-active,
.list-leave-active {
  transition: all 0.5s ease;
}
.list-enter-from,
.list-leave-to {
  opacity: 0;
  transform: translateX(300px);
}
</style>
