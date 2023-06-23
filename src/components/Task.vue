<template>
  <div :class="[task.reminder ? 'reminder' : '', 'task']">
    <h3>
      {{ task.text }}
      <i @click="onDelete(task.id)" class="fas fa-times"></i>
    </h3>
    <p>{{ task.day }}</p>
  </div>
</template>

<script lang='ts'>
import { ITask } from '@/interfaces';
import { defineComponent, PropType } from 'vue';

export default defineComponent({
  name: 'TaskItem',
  props: {
    task: {
      type: Object as PropType<ITask>,
      required: true
    }
  },
  methods: {
    onDelete(id: number) {
      this.$emit('delete-task', id)
    }
  }
})
</script>

<style scope>
.fas {
  color: red;
}

.task {
  background: #f4f4f4;
  margin: 5px;
  padding: 10px 20px;
  cursor: pointer;
}

.task.reminder {
  border-left: 5px solid green;
}

.task h3 {
  display: flex;
  align-items: center;
  justify-content: space-between;
}
</style>
