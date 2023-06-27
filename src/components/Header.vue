<template>
  <header>
    <h1>{{ title }}</h1>
    <Button
      v-show="homePage"
      :text="showAddTask ? 'Close' : 'Add Task'"
      :color="showAddTask ? 'red' : 'green'"
      @btn-click="$emit('toggle-add-task')"
    />
  </header>
</template>

<script lang='ts'>
import { defineComponent } from 'vue';
import Button from './Button.vue';

export default defineComponent({
  name: 'AppHeader',
  props: {
    showAddTask: Boolean,
    title: String
  },
  components: { Button },
  computed: {
    homePage() {
      return this.$route.path === '/'
    }
  }
})
</script>

<style scoped>
  header {
    display: flex;
    justify-content: space-between;
    align-items: center;
    margin-bottom: 20px;
  }
</style>
