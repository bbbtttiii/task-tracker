<template>
  <div class="container">
    <Header title="Task Tracker" />
    <AddTask @add-task="addTask" />
    <Tasks
      @toggle-reminder="toggleReminder"
      @delete-task="deleteTask"
      :tasks="tasks"
    />
  </div>
</template>

<script>
  import Header from './components/Header'
  import Tasks from './components/Tasks'
  import AddTask from './components/AddTask'

  export default {
    name: "App",
    components: {
      Header,
      Tasks,
      AddTask,
    },
    data() {
      return {
        tasks: []
      }
    },
    methods: {
      // add task, spreading existing tasks
      addTask(task) {
        this.tasks = [...this.tasks, task]
      },
      deleteTask(id) {
        if (confirm('Delete task?')) {
          this.tasks = this.tasks.filter((t) => t.id !== id)
        }
      },
      toggleReminder(id) {
        this.tasks = this.tasks.map((t) =>
          t.id === id ? {...t, reminder: !t.reminder} : t
        )
      },
    },
    //using lifecycle method
    created() {
      this.tasks = [
        {
          id: 1,
          text: 'Dentist Appointment',
          day: '3/19 @ 2:30pm',
          reminder: true,
        },
        {
          id: 2,
          text: 'Meeting',
          day: '3/20 @ 1:00pm',
          reminder: true,
        },
        {
          id: 3,
          text: 'Grocery Shopping',
          day: '3/21 @ 10:00am',
          reminder: false,
        }
      ]
    }
  };
</script>

<style>
  @import url("https://fonts.googleapis.com/css2?family=Poppins:wght@300;400&display=swap");

  * {
    box-sizing: border-box;
    margin: 0;
    padding: 0;
  }

  body {
    font-family: "Poppins", sans-serif;
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
