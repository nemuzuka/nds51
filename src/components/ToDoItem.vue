<template>
  <tr @click.stop="openModalDialog">
    <td>{{index + 1}}</td>
    <td class="text-left">{{todo.context}}</td>
    <td>{{todo.lastUpdateDate}}</td>
    <td>
      <button type="button" class="btn btn-danger btn-xs" @click.stop="deleteItem">
        <i class="glyphicon glyphicon-remove"></i>
      </button>
    </td>
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
