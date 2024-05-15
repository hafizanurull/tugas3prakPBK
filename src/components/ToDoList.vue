<template>
  <div>
    <input v-model="newTask" @keyup.enter="addTask" placeholder="Add a new task" />
    <ul>
      <li v-for="(task, index) in tasks" :key="index">
        <span v-if="!task.editing">{{ task.text }}</span>
        <input v-if="task.editing" v-model="task.text" @keyup.enter="saveTask(task)" />
        <button @click="editTask(task)">Edit</button>
        <button @click="deleteTask(index)">Delete</button>
      </li>
    </ul>
  </div>
</template>

<script>
export default {
  data() {
    return {
      newTask: '',
      tasks: JSON.parse(localStorage.getItem('tasks') || '[]')
    }
  },
  watch: {
    tasks: {
      handler() {
        localStorage.setItem('tasks', JSON.stringify(this.tasks))
      },
      deep: true
    }
  },
  methods: {
    addTask() {
      if (this.newTask.trim()) {
        this.tasks.push({ text: this.newTask.trim(), editing: false })
        this.newTask = ''
      }
    },
    deleteTask(index) {
      this.tasks.splice(index, 1)
    },
    editTask(task) {
      task.editing = true
    },
    saveTask(task) {
      task.editing = false
    }
  }
}
</script>

<style scoped>
input {
  margin-bottom: 10px;
}
button {
  margin-left: 10px;
}
</style>
