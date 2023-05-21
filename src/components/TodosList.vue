<script>
import { library } from '@fortawesome/fontawesome-svg-core'
import { FontAwesomeIcon } from '@fortawesome/vue-fontawesome'
import { faTrashAlt } from '@fortawesome/free-solid-svg-icons'

library.add(faTrashAlt)
export default {
  components: {
    FontAwesomeIcon
  },
  data() {
    return {
      tasks: [],
      title: '',
      errorMessage: ''
    }
  },
  methods: {
    addTask() {
      if (this.title.length === 0) {
        this.errorMessage = 'La tâche doit avoir un nom.'
        return
      }
      this.tasks.push({ id: this.tasks.length + 1, name: this.title, done: false })
      this.title = ''
      this.errorMessage = ''
      this.saveTask()
    },
    deleteTask(taskId) {
      const savedTasks = localStorage.getItem('tasks')
      if (savedTasks) {
        const tasks = JSON.parse(savedTasks)
        const index = tasks.findIndex((task) => task.id === taskId)
        if (index !== -1) {
          tasks.splice(index, 1)
          localStorage.setItem('tasks', JSON.stringify(tasks))
          this.tasks = tasks
        }
      }
    },
    saveTask() {
      localStorage.setItem('tasks', JSON.stringify(this.tasks))
    },
    loadTask() {
      const savedTasks = localStorage.getItem('tasks')
      if (savedTasks) {
        this.tasks = JSON.parse(savedTasks)
      }
    }
  },
  mounted() {
    this.loadTask()
  }
}
</script>

<template>
  <div class="todos">
    <h1>Todo-List</h1>
    <form @submit.prevent="addTask" class="todos__form">
      <div class="todos__fromWrapper">
        <input type="text" id="addTask" name="addTask" v-model="title" class="todos__taskName" />
        <label for="addTask" class="todos__nameLabel">Ajouter une tâche</label>
        <button type="submit" class="todos__taskSubmit">Ajouter</button>
      </div>
      <p v-if="errorMessage" class="todos__errorMessage">{{ errorMessage }}</p>
    </form>
    <div v-for="task in tasks" :key="task.id" class="todos__tasks">
      <div>
        <div v-if="task.done" class="todos__task">
          <input
            type="checkbox"
            :id="task.id"
            :name="task.id"
            v-model="task.done"
            checked
            class="todos__taskCheck"
          />
          <label :for="task.id" :class="{ 'todos__taskLabel--checked': task.done }">{{
            task.name
          }}</label>
          <font-awesome-icon icon="trash-alt" @click="deleteTask(task.id)" class="todos__trash" />
        </div>
        <div v-else class="todos__task">
          <input
            type="checkbox"
            :id="task.id"
            :name="task.id"
            v-model="task.done"
            class="todos__taskCheck"
          />
          <label :for="task.id" :class="{ 'todos__taskLabel--checked': task.done }">{{
            task.name
          }}</label>
          <font-awesome-icon icon="trash-alt" @click="deleteTask(task.id)" class="todos__trash" />
        </div>
      </div>
    </div>
  </div>
</template>

<style scoped>
.todos {
  width: 500px;
  padding: 20px;
  border-radius: 15px;
  box-shadow: rgba(100, 100, 111, 0.2) 0px 7px 29px 0px;
}

.todos h1 {
  margin-bottom: 15px;
}

.todos__form {
  margin-bottom: 20px;
}

.todos__fromWrapper {
  display: flex;
  position: relative;
}

.todos__errorMessage {
  color: #c20000;
  margin-top: 5px;
}

.todos__taskName {
  height: 30px;
  border-radius: 5px;
  width: 100%;
  border: 1px solid black;
}

.todos__nameLabel {
  display: block;
  position: absolute;
  background-color: white;
  padding: 0 15px;
  left: 20px;
  top: -9px;
  font-size: 14px;
  margin-bottom: 15px;
}

.todos__taskSubmit {
  position: absolute;
  height: 30px;
  border-radius: 0 5px 5px 0;
  border: none;
  background-color: #00bc77;
  color: white;
  font-weight: bold;
  right: 0;
  cursor: pointer;
}

.todos__tasks {
  padding: 0 40px;
}

.todos__task {
  display: flex;
  height: 30px;
  padding: 0 20px;
  margin-top: 12px;
  align-items: center;
  border-radius: 0 15px 0 15px;
  border: 1px solid rgba(137, 148, 153, 0.2);
  background-color: rgba(137, 148, 153, 0.1);
  position: relative;
}

.todos__taskCheck {
  margin-right: 10px;
}

.todos__taskLabel--checked {
  text-decoration: line-through;
  color: rgba(0, 0, 0, 0.7);
}

.todos__trash {
  position: absolute;
  right: 20px;
  cursor: pointer;
  transition: all 0.1s ease;
}

.todos__trash:hover {
  color: #c20000;
}
</style>
