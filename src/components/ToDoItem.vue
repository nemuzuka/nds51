<template>
  <tr @click.stop="openModalDialog">
    <th>{{index + 1}}</th>
    <th>{{todo.context}}</th>
    <th>{{todo.lastUpdateDate}}</th>
    <th><button type="button" class="btn btn-danger btn-xs" @click.stop="deleteItem"><i class="glyphicon glyphicon-remove"></i></button></th>
  </tr>
</template>

<script>
export default {
  name: 'todo-item',
  props: ["todo", "index"],
  methods: {
    openModalDialog(e) {
      const self = this;
      self.$emit('OpenModalDialog', e, self.todo);
    },
    deleteItem(e) {
      const self = this;
      self.$emit('DeleteItem', e, self.todo);
    }
  }
}
</script>

<style scoped>
  tr {
    cursor: pointer;
  }
</style>
