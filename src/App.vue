<template>
  <section class="todoapp">
    <header class="header">
      <h1>todos</h1>
      <input class="new-todo" autofocus autocomplete="true" placeholder="What needs to bo done?" @keyup.enter="addTodo">
    </header>
    <section class="main" v-show="todos.length">
      <input class="toggle-all" type="checkbox" :checked="allChecked" @change="toggleAll({done: !allChecked})">
      <ul class="todo-list">
        <todo v-for="(todo,index) in filteredTodos" :key="index" :todo="todo" @deleteTodo="delTodo"></todo>
      </ul>
    </section>
    <footer class="footer" v-show="todos.length">
      <span class="todo-count">
        <strong>{{ remaining }}</strong>
        {{ remaining | pluralize('item') }} left
      </span>
      <ul class="filters">
        <li v-for="(val,key) in filters">
          <a :href="'#/'+key" :class="{selected:visibility === key}" @click="visibility = key">
            {{key | capitalize}}
          </a>
        </li>
      </ul>
      <button class="clear-completed" v-show="todos.length>remaining" @click="clearCompleted">Clear completed</button>
    </footer>
  </section>
</template>

<script>
import todo from './components/Todo'

const filters = {
  all: todos => todos,
  active: todos => todos.filter(todo => !todo.done),
  completed: todos => todos.filter(todo => todo.done)
}

export default {
  name: 'app',
  data () {
    return {
      todos:[],
      visibility: 'all',
      filters: filters
    }
  },
  components: {
    todo
  },
  computed: {
    allChecked () {
      return this.todos.every(todo => todo.done); 
    },
    filteredTodos () {
      return filters[this.visibility](this.todos)
    },
    remaining () {
      return this.todos.filter(todo => !todo.done).length
    }
  },
  methods: {
    addTodo (e) {
      var text = e.target.value;
      if(text.trim()){
        let todo_item = {
          text:text,
          done:false
        };
        this.todos.push(todo_item);
        e.target.value = '';
      }
    },
    toggleAll ({done}) {
      this.todos.forEach((todo) => {
        todo.done = done;
      })
    },
    delTodo (todo) {
      this.todos.splice(this.todos.indexOf(todo),1)
    },
    clearCompleted () {
      this.todos = this.todos.filter(todo => !todo.done)
    }
  },
  filters: {
    pluralize: (n,w) => n === 1 ? w : (w+'s'),
    capitalize: s => s.charAt(0).toUpperCase() + s.slice(1)
  }
}
</script>

