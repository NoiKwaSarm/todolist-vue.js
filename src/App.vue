<template>
  <div class="container d-flex flex-column justify-content-center align-items-center TodoApp">
    <h1>Todo List with Vue.js and AdonisJS</h1>
    <form @submit.prevent="addTodo" class="AddTodo">
      <input type="text" v-model="newTodo" placeholder="Add new task" required />
      <button type="submit">Add</button>
    </form>
    <div class="d-flex flex-column">
      <div v-for="todo in todos" :key="todo.id">
        <todo-list
          :todo="todo"
          @remove-todo="removeTodo"
          @toggle-completed="toggleTodo"
          @edit-todo="updateTodo"
        />
      </div>
    </div>
  </div>
</template>

<script lang="ts">
import todoList from './components/todo-list.vue'

interface Todo {
  id: number
  title: string
  completed: boolean
}

export default {
  components: {
    todoList
  },
  data() {
    return {
      newTodo: '',
      todos: [] as Todo[]
    }
  },
  methods: {
    async addTodo() {
      const response = await fetch('http://localhost:3333/todos', {
        method: 'POST',
        headers: {
          'Content-Type': 'application/json'
        },
        body: JSON.stringify({
          title: this.newTodo
        })
      })
      const todo = await response.json()
      this.todos.unshift(todo)
      this.newTodo = ''
    },
    async removeTodo(todo: Todo) {
      await fetch(`http://localhost:3333/todos/${todo.id}`, {
        method: 'DELETE'
      })

      this.todos = this.todos.filter((t) => t.id !== todo.id)
    },
    async toggleTodo(todo: Todo) {
      todo.completed = !todo.completed
      await this.updateTodo(todo)
    },
    async fetchTodos() {
      const response = await fetch('http://localhost:3333/todos')
      this.todos = await response.json()
      this.todos.sort((a, b) => b.id - a.id)
    },
    async updateTodo(todo: Todo) {
      await fetch(`http://localhost:3333/todos/${todo.id}`, {
        method: 'PATCH',
        headers: {
          'Content-Type': 'application/json'
        },
        body: JSON.stringify({
          title: todo.title,
          completed: todo.completed
        })
      })

      this.todos = this.todos.map((t) => {
        if (t.id === todo.id) {
          return todo
        }
        return t
      })
    }
  },
  async created() {
    await this.fetchTodos()
  }
}
</script>

<style scoped>
.TodoApp {
  margin-top: 20px;
}

.AddTodo {
  display: flex;
  margin: 10px;
}

.AddTodo input {
  padding: 10px;
  width: 500px;
  border: 1px solid #e0e0e0;
  border-radius: 10px 0 0 10px;
}

.AddTodo button {
  padding: 10px 20px;
  border: 1px solid #e0e0e0;
  border-radius: 0 10px 10px 0;
  background-color: #f0f0f0;
}
</style>
