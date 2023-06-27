<template>
  <h3>Version 1.0.0</h3>
  <router-link to="/">Go Back</router-link>
</template>

<script lang="ts">
import { defineComponent } from 'vue'

export default defineComponent({
  name: 'AboutView',
  props: {
    showAddTask: Boolean
  },
  emits: ['close-task-form']
})
</script>
