<template>
  <div class="container">
    <Header @show-Add-Task="showaddTask" title="Task Tracker" :toggleAddTask="showAddTask" />
    <div class="" v-show="showAddTask">
      <AddTask @add-task="addTask" />
    </div>
    <Tasks @toggle-reminder="togglereminder" @delete-task="deleteTask" :tasks="tasks" />
  </div>
</template>

<script>
import Header from './components/Header.vue'
import Tasks from './components/Tasks.vue';
import AddTask from './components/AddTask.vue';

export default {
  name: 'App',
  components: {
    Header,
    Tasks,
    AddTask
  },
  data() {
    return {
      tasks: [],
      showAddTask: true
    }
  },

  async created() {
    this.tasks = await this.fetchDatas()
  },
  methods: {
    async fetchDatas() {
      const res = await fetch("http://localhost:5000/tasks/")
      const data = await res.json()
      return data
    },
    async fetchData(id) {
      const res = await fetch(`http://localhost:5000/tasks/${id}`)
      const data = await res.json()
      return data
    },
    showaddTask() {
      this.showAddTask = !this.showAddTask
    },
    async addTask(task) {
      const res = await fetch(`http://localhost:5000/tasks`, {
        method: "POST",
        headers: {
          "Content-Type": "application/json",
        },
        body: JSON.stringify(task),
      })
      const data = await res.json()
      this.tasks = [...this.tasks, data]
    },
    async deleteTask(id) {
      if (confirm("Do you really want to delete this item?")){
       const res = await fetch(`http://localhost:5000/tasks/${id}`, {
        method: "DELETE"
      })
        res.status === 200 ?(this.tasks = this.tasks.filter(task => task.id !== id)):alert('Error deleting task!')
      }

    },
    async togglereminder(id) {
      const toggleReminder = await this.fetchData(id)
      const updatedTask = {...toggleReminder, reminder:!toggleReminder.reminder}
      const res = await fetch(`http://localhost:5000/tasks/${id}`,{
        method:"PUT",
        headers:{
          "Content-Type": "application/json",
        },
        body: JSON.stringify(updatedTask)
      })
      const data = await res.json()
      this.tasks = this.tasks.map(task => task.id === id ? { ...task, reminder: data.reminder } : task)
    }
  },
}
</script>


<style>
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
