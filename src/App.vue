<template>
  <main>
    <!-- heading -->
    <header>
      <img src="./assets/pinia-logo.svg" alt="pinia logo" />
      <h1>Pinia Task</h1>
    </header>

    <!-- new task form -->
    <div class="new-task-form">
      <TaskForm />
    </div>

    <!-- filters -->
    <nav class="filter">
      <button @click="filer = 'all'">All tasks</button>
      <button @click="filer = 'favs'">Fav tasks</button>
    </nav>

    <!-- loading -->
    <div class="loading" v-if="loading">Loadig tasks...</div>

    <!-- Task list -->
    <div class="task-list" v-if="filer === 'all'">
      <p>You have {{ totalCount }} tasks left to do</p>
      <div v-for="task in tasks" :key="task.id">
        <TaskDetails :task="task" />
      </div>
    </div>

    <!-- Task favs list -->
    <div class="task-list" v-if="filer === 'favs'">
      <p>You have {{ favCount }} favs left to do</p>
      <div v-for="task in favs" :key="task.id">
        <TaskDetails :task="task" />
      </div>
    </div>

    <!-- reset -->
    <button @click="taskStore.$reset">reset</button>
  </main>
</template>

<script>
import { ref } from "vue";
import { useTaskStore } from "@/stores/TaskStore";
import TaskDetails from "./components/TaskDetails.vue";
import TaskForm from "./components/TaskForm.vue";
import { storeToRefs } from "pinia";

export default {
  components: { TaskDetails, TaskForm },
  setup() {
    const taskStore = useTaskStore();

    const { tasks, loading, favs, totalCount, favCount } =
      storeToRefs(taskStore);

    // fetch task
    taskStore.getTasks();

    const filer = ref("all");

    return { taskStore, filer, tasks, loading, favs, totalCount, favCount };
  },
};
</script>