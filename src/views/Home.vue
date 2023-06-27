<template>
  <AddTask @add-task="addTask" v-if="showAddTask" />
  <Tasks @delete-task="deleteTask" @toggle-reminder="toggleReminder" :tasks="tasks" />
</template>

<script lang="ts">
import { defineComponent } from 'vue'
import AddTask from '../components/AddTask.vue';
import Tasks from '../components/Tasks.vue';
import { ITask } from '@/interfaces';

export default defineComponent({
  name: 'HomeView',
  components: {
    AddTask,
    Tasks
  },
  props: {
    showAddTask: Boolean
  },
  data() {
    return {
      tasks: [] as ITask[]
    }
  },
  methods: {
    async addTask(newTask: ITask) {
      const res = await fetch('api/tasks', {
        method: 'POST',
        headers: {
          'Content-type': 'application/json'
        },
        body: JSON.stringify(newTask)
      })
      const task: ITask = await res.json()
      this.tasks = [...this.tasks, task]
      this.$emit('close-task-form')
    },
    async deleteTask(id: number) {
      if (!confirm('Are you sure?')) return

      const res = await fetch(`api/tasks/${id}`, { method: 'DELETE' })
      if (res.status !== 200) {
        alert('Error deleting task')
        return
      }
      this.tasks = this.tasks.filter((task: ITask) => task.id !== id)
    },
    async toggleReminder(id: number) {
      const taskToToggle = this.tasks.find((task: ITask) => task.id === id)
      if (taskToToggle === undefined) return alert(`"${id}" does not exist. Please reload the page.`)

      const updTask = { ...taskToToggle, reminder: !taskToToggle.reminder }
      const data = await this.updateTask(id, updTask)
      this.tasks = this.tasks.map((task: ITask) =>
        task.id === id ? { ...task, reminder: data.reminder } : task
      )
    },
    async fetchTasks() {
      const res = await fetch(`api/tasks`)
      const tasks: ITask[] = await res.json()
      return tasks
    },
    async updateTask(id: number, task: ITask) {
      const res = await fetch(`api/tasks/${id}`, {
        method: 'PUT',
        headers: {
          'Content-type': 'application/json'
        },
        body: JSON.stringify(task)
      })
      const data: ITask = await res.json()
      return data
    }
  },
  async created() {
    this.tasks = await this.fetchTasks()
  },
  emits: ['close-task-form']
})
</script>
