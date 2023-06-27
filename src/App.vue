<template>
  <div class="container">
    <Header title="Task Tracker" @toggle-add-task="toggleAddTask" :showAddTask="showAddTask" />
    <AddTask @add-task="addTask" v-if="showAddTask" />
    <Tasks @delete-task="deleteTask" @toggle-reminder="toggleReminder" :tasks="tasks" />
  </div>
</template>

<script lang="ts">
import { defineComponent } from 'vue';
import AddTask from './components/AddTask.vue';
import Header from './components/Header.vue';
import Tasks from './components/Tasks.vue';
import { ITask } from '@/interfaces';

export default defineComponent({
  name: 'App',
  components: {
    AddTask,
    Header,
    Tasks
  },
  data() {
    return {
      showAddTask: false,
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
      this.showAddTask = false
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
      const res = await fetch(`api/tasks/${id}`, {
        method: 'PUT',
        headers: {
          'Content-type': 'application/json'
        },
        body: JSON.stringify(updTask)
      })
      const data = await res.json()
      this.tasks = this.tasks.map((task: ITask) =>
        task.id === id ? { ...task, reminder: data.reminder } : task
      )
    },
    toggleAddTask() {
      this.showAddTask = !this.showAddTask
    },
    async fetchTasks() {
      const res = await fetch(`api/tasks`)
      const tasks: ITask[] = await res.json()
      return tasks
    },
    async fetchTask(id: number) {
      const res = await fetch(`api/tasks/${id}`)
      const task: ITask = await res.json()
      return task
    }
  },
  async created() {
    this.tasks = await this.fetchTasks()
  }
});
</script>

<style>
@import url('https://fonts.googleapis.com/css2?family=Poppins:wght@300;400&display=swap');

* {
  box-sizing: border-box;
  margin: 0;
  padding: 0;
}

body {
  font-family: 'Poppins', sans-serif;
}

.container {
  max-width: 500px;
  margin: 30px auto;
  overflow: auto;
  min-height: 300px;
  border: 1px solid steelblue;
  padding: 30px;
  border-radius: 5px;
}

.btn {
  display: inline-block;
  background: #000;
  color: #fff;
  border: none;
  padding: 10px 20px;
  margin: 5px;
  border-radius: 5px;
  cursor: pointer;
  text-decoration: none;
  font-size: 15px;
  font-family: inherit;
}

.btn:focus {
  outline: none;
}

.btn:active {
  transform: scale(0.98);
}

.btn-block {
  display: block;
  width: 100%;
}
</style>
