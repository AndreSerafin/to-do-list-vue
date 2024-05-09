<template>
  <div
    class="min-w-screen min-h-screen flex flex-col items-center bg-slate-800"
  >
    <div class="max-w-[600px] w-full mt-14 flex flex-col gap-4 px-4">
      <h1 class="text-3xl text-slate-200 font-bold text-center">Todo list</h1>
      <form class="flex flex-1 gap-2" @submit.prevent="addTask">
        <input
          v-model="newTask"
          class="flex-1 rounded-md bg-slate-300 h-11 pl-4 font-medium placeholder:text-slate-500"
          type="text"
          placeholder="Nova tarefa"
        />
        <button
          type="submit"
          class="bg-sky-800 flex justify-center rounded-md items-center text-slate-100 basis-16"
          title="Add task"
        >
          <ph-plus-circle size="1.5rem" />
        </button>
      </form>

      <div class="max-w-[600px] w-full flex flex-col gap-2">
        <div
          v-for="(task, index) in tasks"
          :key="index"
          class="bg-slate-600 px-2 py-4 flex items-center gap-2 rounded-md"
        >
          <CheckBox
            :checked="task.completed"
            @update:checked="completeTask(index)"
          />
          <span
            :class="{ 'line-through': task.completed }"
            class="text-white flex-1"
            >{{ task.name }}</span
          >
          <button
            class="text-slate-300 hover:text-red-500 transition-all"
            @click="removeTask(index)"
          >
            <ph-trash size="1.2rem" />
          </button>
        </div>
      </div>

      <div v-if="tasks.length === 0" class="max-w-[600px] h-[300px] w-full flex flex-col justify-center items-center font-semibold text-slate-500">
        <ph-clipboard size="6rem" />
        <p class="font-semibold">Nenhuma tarefa cadastrada</p>
      </div>
    </div>
  </div>
</template>

<script>
import CheckBox from "./components/check-box.vue";

export default {
  components: {
    CheckBox,
  },
  data() {
    return {
      newTask: "",
      tasks: [],
    };
  },
  mounted() {
    this.loadTasksFromLocalStorage();
  },
  methods: {
    addTask() {
      if (this.newTask.trim() !== "") {
        this.tasks.push({ name: this.newTask, completed: false });
        this.saveTasksToLocalStorage();
        this.newTask = "";
      }
    },
    removeTask(index) {
      this.tasks.splice(index, 1);
      this.saveTasksToLocalStorage();
    },
    completeTask(index) {
      this.tasks[index].completed = !this.tasks[index].completed;
      this.saveTasksToLocalStorage();
    },
    saveTasksToLocalStorage() {
      localStorage.setItem("tasks", JSON.stringify(this.tasks));
    },
    loadTasksFromLocalStorage() {
      const savedTasks = localStorage.getItem("tasks");
      if (savedTasks) {
        this.tasks = JSON.parse(savedTasks);
      }
    },
  },
};
</script>
