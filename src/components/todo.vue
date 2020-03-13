<template>
    <section class="todoapp">
        <header>
            <h1>TODO</h1>
            <input type="text" class="new-todo" placeholder="Ajouter une tâche" v-model="newTodo" @keyup.enter="addTodo">
        </header>

        <div class="main">
            <input id="toggle-all" class="toggle-all" type="checkbox" v-model="allDone">
            <label for="toggle-all"></label>
            <ul class="todo-list">
                <li class="todo" v-for="todo in filteredTodos" :key="todo.name" :class="{completed: todo.completed, editing: todo == editing}">
                    <div class="view">
                        <input type="checkbox" v-model="todo.completed" class="toggle">
                        <label @dblclick="editTodo(todo)" >{{ todo.name}}</label>
                        <button class="destroy" @click.prevent="deleteTodo(todo)"></button>
                    </div>
                    <input type="text" class="edit" v-model="todo.name" @keyup.enter="doneEdit" @blur="doneEdit" v-focus="todo == editing">
                </li>
            </ul>
        </div>
        <footer class="footer" v-show="todos.length > 0">
            <span class="todo-count"><strong>{{ remaining }}</strong> tâches à faire</span>
            <ul class="filters">
                <li><a href="#" :class="{selected: filter === 'all'}" @click.prevent="filter ='all'">Toutes</a></li>
                <li><a href="#" :class="{selected: filter === 'todo'}" @click.prevent="filter ='todo'">A Faire</a></li>
                <li><a href="#" :class="{selected: filter === 'done'}" @click.prevent="filter ='done'">Faites</a></li>
            </ul>
            <button class="clear-completed" v-show="completed" @click.prevent="deleteCompleted">Clear All Finish</button>
        </footer>
    </section>
</template>

<script>
import Vue from 'vue'

export default {
  data () {
    return {
      todos: [],
      newTodo: '',
      filter: 'all',
      editing: null
    }
  },
  methods: {
    addTodo () {
      this.todos.push({
        completed: false,
        name: this.newTodo
      })
      this.newTodo = ''
    },
    deleteTodo (todo) {
      this.todos = this.todos.filter(i => i !== todo)
    },
    deleteCompleted () {
      this.todos = this.todos.filter(todo => !todo.completed)
    },
    editTodo (todo) {
      this.editing = todo
    },
    doneEdit: function (todo) {
      if (!this.editing) {
        return
      }
      this.editing = null
      todo.title = todo.title.trim()
      if (!todo.title) {
        this.deleteTodo(todo)
      }
    }

  },
  computed: {
    allDone: {
      get () {
        return this.remaining === 0
      },
      set (value) {
        this.todos.forEach(todo => {
          todo.completed = value} )}
    },
    remaining () { return this.todos.filter(todo => !todo.completed).length },
    completed () { return this.todos.filter(todo => todo.completed).length },
    filteredTodos () {
      if (this.filter === 'todo') {
        return this.todos.filter(todo => !todo.completed)
      } else if (this.filter === 'done') {
        return this.todos.filter(todo => todo.completed)
      }
      return this.todos
    }
  },
  directives: {
    focus (el, value) {
      if (value) {
        Vue.nextTick(_ => {
        el.focus()
        })
      }
    }
  }
}
</script>

<style src="./todo.css"></style>