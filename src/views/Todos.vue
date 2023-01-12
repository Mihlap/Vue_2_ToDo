<template>
  <div>
    <h2>Приложение для достижения цели</h2>
    <router-link to="/">Перейти на главную страницу</router-link>
    <hr>
    <AddTodo
    v-on:add-todo="addTodo" 
    />
    <select v-model="filter">
      <option value="all">Все</option>
      <option value="completed">Выполненные</option>
      <option value="not-completed">В процессе выполнения</option>
    </select>
    <hr>
    <Loader
    v-if="loading"
    />
    <TodoList
    v-else-if="getFilterTodo.length"
     v-bind:todos="getFilterTodo"
     v-on:remove-todo="removeTodo"
      />
      <p v-else>Пожалуйста, добавь свои цели</p>
   </div>
</template>

<script>
import TodoList from '@/components/TodoList';
import AddTodo from '@/components/AddTodo';
import Loader from '@/components/Loader';
export default {
  name: 'App',
  data() {
    return {
      todos: [],
      loading: true,
      filter: 'all'
    }
  },
  mounted() {
    fetch('https://jsonplaceholder.typicode.com/todos?_limit=5')
      .then(response => response.json())
      .then(json => {
        setTimeout(() => {

          this.todos = json
        this.loading = false
        }, 500)
        
      })
  },
//  
  computed: {
    getFilterTodo() {
      if (this.filter === 'all') {
      return this.todos
      }
      if (this.filter === 'completed') {
      return this.todos.filter(el => el.completed)
      }
      if (this.filter === 'not-completed') {
      return this.todos.filter(el => !el.completed)
    }
  }
},
  methods: {
    removeTodo(id) {
this.todos = this.todos.filter(el => el.id !== id) 
    },
    addTodo(todo) {
this.todos.push(todo)
    }
  },
  components: {
    TodoList: TodoList,
    AddTodo: AddTodo,
    Loader: Loader,
  }
}
</script>