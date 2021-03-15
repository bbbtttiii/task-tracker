<template>
  <AddTask
    v-show="showAddTask"
    @add-task="addTask"
  />
  <Tasks
    @toggle-reminder="toggleReminder"
    @delete-task="deleteTask"
    :tasks="tasks"
  />
</template>

<script>
  import Tasks from '../components/Tasks'
  import AddTask from '../components/AddTask'

  export default {
    name: 'Home',
    props: {
      showAddTask: Boolean,
    },
    components: {
      Tasks,
      AddTask
    },
    data() {
      return {
        tasks: [],
      }
    },
    methods: {
      async addTask(task) {
        const res = await fetch('api/tasks', {
          method: 'POST',
          headers: {
            'Content-type': 'application/json',
          },
          body: JSON.stringify(task)
        })
        const data = await res.json()
        // add task, spreading existing tasks
        this.tasks = [...this.tasks, data]
      },

      async deleteTask(id) {
        if (confirm('Delete task?')) {
          const res = await fetch(`api/tasks/${id}`, {
            method: 'DELETE'
          })

          if (res.status === 200) {
            this.tasks = this.tasks.filter((t) => t.id !== id)
          } else {
            alert('Error')
          }
        }
      },

      async toggleReminder(id) {
        const taskToToggle = await this.fetchTask(id)
        const updatedTask = {...taskToToggle, reminder: !taskToToggle.reminder} 
      
        const res = await fetch(`api/tasks/${id}`, {
          method: 'PUT',
          headers: {
            'Content-type': 'application/json'
          },
          body: JSON.stringify(updatedTask)
        })

        const data = await res.json()

        this.tasks = this.tasks.map((t) =>
          t.id === id ? {...t, reminder: data.reminder} : t
        )
      },

      // fetch data from json server using proxy
      async fetchTasks() {
        const res = await fetch('api/tasks')
        const data = await res.json()
        return data
      },

      async fetchTask(id) {
        const res = await fetch(`api/tasks/${id}`)
        const data = await res.json()
        return data
      },
    },
    //lifecycle method: runs when instance is created
    async created() {
      this.tasks = await this.fetchTasks()
    }
  }
</script>