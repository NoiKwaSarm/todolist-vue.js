<template>
  <div
    class="todolist d-flex flex-row justify-content-between align-items-center"
    :class="{ 'completed-list': completed }"
  >
    <div class="d-flex flex-row align-items-center">
      <input class="checkbox" type="checkbox" v-model="completed" @change="toggleComplete" />
      <span :class="{ completed: completed }">{{ todo.title }}</span>
    </div>
    <div class="d-flex flex-row align-items-center">
      <button @click="removeTodo" class="remove-btn">Delete</button>
    </div>
  </div>
</template>

<script lang="ts">
export default {
  props: ['todo'],
  data() {
    return {
      completed: this.todo.completed
    }
  },
  methods: {
    async removeTodo() {
      this.$emit('remove-todo', this.todo)
    },
    async toggleComplete() {
      this.$emit('toggle-completed', this.todo)
    },
    async editTodo() {
      this.$emit('edit-todo', this.todo)
    }
  }
}
</script>

<style scoped>
.todolist {
  padding: 10px;
  min-width: 500px;
  border-bottom: 1px solid #e0e0e0;
}

.completed-list {
  background-color: #f0f0f0;
}

.completed {
  text-decoration: line-through;
}

.checkbox {
  margin-right: 10px;
  -webkit-appearance: none;
  height: 25px;
  width: 25px;
  transition: 0.3s;
  background-color: #a3a3a3;
  text-align: center;
  font-weight: 600;
  color: white;
  border-radius: 3px;
  outline: none;
  font-family: Arial, Helvetica, sans-serif;
}

.checkbox:checked {
  background-color: #0e9700;
}

/* .checkbox:before {
  content: '✖';
} */

.checkbox:checked:before {
  content: '✔';
}

.checkbox:hover {
  cursor: pointer;
  background-color: rgb(128, 128, 128);
}

.checkbox:checked:hover {
  background-color: rgb(0, 128, 0);
}

.remove-btn {
  margin-left: 10px;
  border-radius: 30px;
  background-color: #ff0000;
  color: white;
  border: none;
  padding: 5px 10px;
  transition: 0.3s;
  font-weight: 600;
}

.remove-btn:hover {
  background-color: #ff3333;
  transform: scale(1.05);
}
</style>
