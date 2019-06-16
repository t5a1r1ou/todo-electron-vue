<template>
  <div class="edit">
    <h1>TODO</h1>
    <h2>一覧・編集</h2>
    <table>
        <tr v-for="item in todos" :key="item.id">
          <td>{{ item.comment }}</td>
          <td class="button">
            <button v-on:click="doRemove(item)">
              削除
            </button>
          </td>
        </tr>
    </table>

    <h2>新しい作業の追加</h2>
    <form action="" class="add-form" v-on:submit.prevent="doAdd">
      作業内容 <input type="text" ref="comment">
      <button type="submit">追加</button>
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
    doAdd: function(event, value) {
      var comment = this.$refs.comment
      if (!comment.value.length) {
        return
      }
      this.todos.push({
        id: todoStorage.uid++,
        comment: comment.value,
      })
      comment.value = ''
    },
    doRemove: function(item) {
      var index = this.todos.indexOf(item)
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
</style>
