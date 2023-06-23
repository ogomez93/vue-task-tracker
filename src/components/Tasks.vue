<template>
  <div :key="task.id" v-for="task in tasks">
    <Task @delete-task="$emit('delete-task', task.id)" :task="task" />
  </div>
</template>

<script lang='ts'>
import { defineComponent, PropType } from 'vue';
import Task from './Task.vue';
import { ITask } from '@/interfaces';

export default defineComponent({
  name: 'TaskList',
  props: {
    tasks: Array as PropType<ITask[]>
  },
  components: {
    Task
  },
  emits: ['delete-task']
})
</script>
