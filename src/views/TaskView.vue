<script setup>
import { ref, watch, computed } from "vue";
import { uid } from "uid";
import { Icon } from "@iconify/vue";
import TaskCreator from "../components/TaskCreator.vue";
import TaskItem from "../components/TaskItem.vue";
const taskList = ref([]);

watch(taskList, () => {
  setTaskListToLocalStorage()
}, {
  deep: true
})

const allTasksCompleted = computed(() => {
  return taskList.value.every((task) => task.isCompleted)
})

const fetchTaskList = () => {
  const savedTaskList = JSON.parse(localStorage.getItem("taskList"));
  if (savedTaskList) {
    taskList.value = savedTaskList;
  }
};

fetchTaskList();

const setTaskListToLocalStorage = () => {
  localStorage.setItem("taskList", JSON.stringify(taskList.value));
};

const createTask = (task) => {
  taskList.value.push({
    id: uid(),
    task,
    isCompleted: null,
    isEditing: null,
  });
};
const toggleTaskComplete = (taskPos) => {
  taskList.value[taskPos].isCompleted = !taskList.value[taskPos].isCompleted;
};
const toggleEditTask = (taskPos) => {
  taskList.value[taskPos].isEditing = !taskList.value[taskPos].isEditing;
};
const updateTask = (taskVal, taskPos) => {
  taskList.value[taskPos].task = taskVal;
};
const deleteTask = (taskId) => {
  taskList.value = taskList.value.filter((task) => task.id !== taskId);
};
</script>

<template>
  <main>
    <h1>Create Task</h1>
    <TaskCreator @create-task="createTask" />
    <ul class="task-list" v-if="taskList.length > 0">
      <TaskItem
        v-for="(task, index) in taskList"
        :task="task"
        :index="index"
        v-bind:key="task.id"
        @toggle-complete="toggleTaskComplete"
        @edit-task="toggleEditTask"
        @update-task="updateTask"
        @delete-task="deleteTask"
      />
    </ul>
    <p v-else class="task-msg">
      <Icon icon="noto-v1:sad-but-relieved-face" />
      <span>You have no tasks to complete! Add one!</span>
    </p>
    <p v-if="allTasksCompleted && taskList.length" class="completed-msg">
      <Icon icon="noto-v1:party-popper" />
      <span>You have completed all your tasks!</span></p>
  </main>
</template>

<style lang="scss" scoped>
main {
  display: flex;
  flex-direction: column;
  max-width: 500px;
  width: 100%;
  margin: 0 auto;
  padding: 40px 16px;

  h1 {
    margin-bottom: 16px;
    text-align: center;
  }

  .task-list {
    display: flex;
    flex-direction: column;
    list-style: none;
    margin-top: 24px;
    gap: 20px;
  }

  .completed-msg {
    display: flex;
    align-items: center;
    justify-content: center;
    gap: 8px;
    margin-top: 24px;
  }
}
</style>
