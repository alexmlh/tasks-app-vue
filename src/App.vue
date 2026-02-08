<script lang="ts" setup>
import { ref, computed } from "vue";
import TaskForm from "./components/TaskForm.vue";
import TaskList from "./components/TaskList.vue";
import FilterButton from "./components/FilterButton.vue";
import type { Task } from "./types";
import type { TasksFilter } from "./types";

const tasks = ref<Task[]>([]);
const filter = ref<TasksFilter>("all");
const filters: TasksFilter[] = ["all", "active", "completed"];

const filteredTasks = computed(() => {
  switch (filter.value) {
    case "all":
      return tasks.value;
    case "completed":
      return tasks.value.filter((task) => task.done === true);
    case "active":
      return tasks.value.filter((task) => task.done === false);
  }
  return tasks.value;
});

function addTask(newTask: string) {
  tasks.value.push({ id: crypto.randomUUID(), title: newTask, done: false });
}

function toggleDone(id: string) {
  const task = tasks.value.find((task) => task.id === id);
  if (task) {
    task.done = !task.done;
  }
}

function removeTask(id: string) {
  const index = tasks.value.findIndex((task) => task.id == id);
  if (index !== -1) tasks.value.splice(index, 1);
}

function setFilter(value: TasksFilter) {
  filter.value = value;
}
</script>

<template>
  <main>
    <h1>Tasks App</h1>
    <TaskForm @add-task="addTask" />
    <h3 v-if="!tasks.length">Add a tast to get started.</h3>
    <h3 v-else>
      {{ tasks.filter((task) => task.done).length }} / {{ tasks.length }} tasks
      completed
    </h3>
    <div class="button-container" v-if="tasks.length">
      <FilterButton
        v-for="f in filters"
        :key="f"
        :currentFilter="filter"
        :filter="f"
        @set-filter="setFilter"
      />
    </div>

    <TaskList
      :tasks="filteredTasks"
      @toggle-done="toggleDone"
      @remove-task="removeTask"
    />
  </main>
</template>

<style>
main {
  max-width: 800px;
  background-color: #fff;
  margin: 1rem auto;
}
.button-container {
  display: flex;
  justify-content: end;
  gap: 0.5rem;
}
</style>
