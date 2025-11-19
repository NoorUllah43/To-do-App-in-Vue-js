<script setup>
import { ref, onMounted } from "vue";

const tasks = ref([]);

onMounted(() => {
  const saved = localStorage.getItem("tasks");
  if (saved) {
    tasks.value = JSON.parse(saved);
  }
});

const AddNewTask = () => {
  const taskInput = document.getElementById("taskInput");
  const taskValue = taskInput.value.trim();
  if (taskValue) {
    const newTask = {
      id: tasks.value.length + 1,
      text: taskValue,
      completed: false,
    };
    tasks.value.push(newTask);
    localStorage.setItem("tasks", JSON.stringify(tasks.value));
    taskInput.value = "";
  }
};

const DeleteTask = () => {
  const taskItem = event.target.parentElement;
  const taskId = parseInt(taskItem.getAttribute("data-id"));
  tasks.value = tasks.value.filter((task) => task.id !== taskId);
  localStorage.setItem("tasks", JSON.stringify(tasks.value));
};
</script>

<template>
  <div id="app">
    <h1>To-Do App</h1>
    <div class="add-task">
      <input type="text" id="taskInput" placeholder="Add a new task" />
      <button @click="AddNewTask">Add Task</button>
    </div>
    <ul class="task-list">
      <li v-for="task in tasks" :key="task.id" :data-id="task.id">
        <input type="checkbox" v-model="task.completed" />
        <span
          :style="{ textDecoration: task.completed ? 'line-through' : 'none' }"
          >{{ task.text }}</span
        >
        <button @click="DeleteTask">Delete</button>
      </li>
    </ul>
  </div>
</template>

<style scoped>
#app {
  text-align: center;
  width: 400px;
  padding: 20px;
  background-color: #333;
  border-radius: 8px;
  box-shadow: 0 4px 8px rgba(0, 0, 0, 0.2);
}
.add-task {
  margin-bottom: 20px;
  display: flex;
  justify-content: center;
  align-items: center;
}
.add-task input {
  width: 70%;
  height: 30px;
  padding: 0 10px;
  border: none;
  border-radius: 4px;
  margin-right: 10px;
  outline: none;
  border: 1px solid #28a745;
}
.add-task button {
  height: 34px;
  padding: 0 15px;
  border: none;
  border-radius: 4px;
  background-color: #28a745;
  color: white;
  cursor: pointer;
}
.add-task button:hover {
  background-color: #218838;
}
.task-list {
  list-style-type: none;
  padding: 0;
}
.task-list li {
  display: flex;
  align-items: center;
  justify-content: space-between;
  padding: 10px;
  border-bottom: 1px solid #444;
}
.task-list li span {
  flex-grow: 1;
  margin-left: 10px;
  text-align: left;
}
.task-list li button {
  padding: 5px 10px;
  border: none;
  border-radius: 4px;
  background-color: #dc3545;
  color: white;
  cursor: pointer;
}
.task-list li button:hover {
  background-color: #c82333;
}
</style>
