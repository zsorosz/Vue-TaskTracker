<script setup>
import { ref } from "vue";
import { uid } from "uid";
import { Icon } from "@iconify/vue";
import TaskCreator from "../components/TaskCreator.vue";
import TaskItem from "../components/TaskItem.vue";
const taskList = ref([]);

const createTask = (task) => {
  taskList.value.push({
    id: uid(),
    task,
    isCompleted: null,
    isEditing: null,
  });
};
</script>

<template>
  <main>
    <h1>Create Task</h1>
    <TaskCreator @create-task="createTask" />
    <ul class="task-list" v-if="taskList.length > 0">
      <TaskItem v-for="task in taskList" :task="task" v-bind:key="task.id" />
    </ul>
    <p v-else class="task-msg">
      <Icon icon="noto-v1:sad-but-relieved-face" />
      <span>You have no tasks to complete! Add one!</span>
    </p>
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

  .todo-list {
    display: flex;
    flex-direction: column;
    list-style: none;
    margin-top: 24px;
    gap: 20px;
  }

  .task-msg {
    display: flex;
    align-items: center;
    justify-content: center;
    gap: 8px;
    margin-top: 24px;
  }
}
</style>
