<script setup>
  import { ref } from 'vue';
  import {uid} from 'uid';
  import TaskCreator from '../components/TaskCreator.vue';
  const taskList = ref([]);

  const createTaks = (task) => {
    taskList.value.push({
      id: uid(),
      task,
      isCompleted: null,
      isEditing: null
    })
  }
</script>

<template>
  <main>
    <h1>Create Task</h1>
    <TaskCreator @create-task="createTaks()" />
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
  }
</style>
