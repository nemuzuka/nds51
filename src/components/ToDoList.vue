<template>

  <div>
    <table class="table table-bordered table-hover table-condensed" v-if="todos.length > 0">
      <thead>
      <tr>
        <th style="width: 30px">#</th>
        <th>ToDo</th>
        <th style="width: 180px">最終更新日時</th>
        <th style="width: 30px"></th>
      </tr>
      </thead>
      <tbody>
        <todo-item v-for="(todo, index) in todos" :todo="todo" :index="index" :key="todo.id" @OpenModalDialog="openModalDialog" @DeleteItem="deleteItem"></todo-item>
      </tbody>
    </table>

    <div v-if="todos.length <= 0">
      <div class="alert alert-danger" role="alert">表示するToDoがありません...</div>
    </div>

  </div>

</template>

<script>
import TodoItem from './ToDoItem'

export default {
  name: 'todo-list',
  props: ["todos"],
  components: {
    'todo-item': TodoItem
  },
  methods: {
    openModalDialog(e, todo) {
      this.$emit('OpenModalDialog', e, todo);
    },
    deleteItem(e, todo) {
      this.$emit('DeleteItem', e, todo);
    }
  }
}
</script>

<style scoped>
.table thead {
  background-color: #6f5499;
  color: #fff;
}
</style>
