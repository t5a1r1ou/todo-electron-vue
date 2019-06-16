<template>
  <div class="edit">
    <form action="" v-on.submit.prevent="doneAdd">
      <h1 v-if="todos[0]">{{ todos[0].comment }}</h1>
      <h1 v-else>TODOを登録してください</h1>
      <div v-if="todos[0]">
        <h2>メモ</h2>
        <textarea name="memo" rows="8" cols="80" ref="memo"></textarea>
      </div>
      <div class="buttons" v-if="todos[0]">
        <button v-on:click="doChangeState(todos[0])" class="button">
          保留
        </button>
        <button v-on:click="doChangeLast(todos[0])" class="button">
          最後に保留
        </button>
        <button v-on:click="doRemove(todos[0])" class="button" type="submit">
          完了
        </button>
      </div>
    </form>
  </div>
</template>

<script>

let STORAGE_KEY = 'todos-vuejs-demo'
let todoStorage = {
  fetch: function() {
    let todos = JSON.parse(
      localStorage.getItem(STORAGE_KEY) || '[]'
    )
    todos.forEach(function(todo, index) {
      todo.id = index
    })
    todoStorage.uid = todos.length
    return todos
  },
  save: function(todos) {
    localStorage.setItem(STORAGE_KEY, JSON.stringify(todos))
  }
}

export default {
  name: 'edit',
  data: () => {
    return {
      todos: []
    }
  },
  methods: {
    doChangeState: function(item) {
      let index = this.todos.indexOf(item)
      this.todos.splice(index, 1)
      this.todos.splice(index + 1, 0, item)
    },
    doChangeLast: function(item) {
      let index = this.todos.indexOf(item)
      let last_index = this.todos.length
      this.todos.splice(index, 1)
      this.todos.splice(last_index, 0, item)
    },
    doRemove: function(item) {
      let index = this.todos.indexOf(item)
      this.todos.splice(index, 1)
    }
  },
  watch: {
    todos: {
      handler: function(todos) {
        todoStorage.save(todos)
      },
      deep: true
    }
  },
  created() {
    this.todos = todoStorage.fetch()
  }
}
</script>

<style media="screen">
table {
  margin: 0 auto;
}

.done {
  text-decoration: line-through;
}

.buttons {
  display: block;
  margin: 0 auto;
}

.button {
  margin: 0 1em;
}
</style>
