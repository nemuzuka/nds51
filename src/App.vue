<template>
  <div class="container">

    <div class="row">
      <div class="col-sm-6">
        <input type="text" class="form-control" placeholder="Search for..." v-model="keyword">
      </div>
    </div>

    <div class="todo-list-area">
      <todo-list :todos="filteredTodos" @OpenModalDialog="openModalDialog" @DeleteItem="deleteTodo"></todo-list>
    </div>

    <p class="save"><a href="javascript:void(0)" @click="openModalDialog"><i class="glyphicon glyphicon-plus"></i></a></p>
    <todo-edit-dialog ref="modalDialog" @StoreTodo="storeTodo"></todo-edit-dialog>

  </div>
</template>

<script>
import TodoList from './components/ToDoList'
import ToDoEditDialog from './components/ToDoEditDialog'

export default {
  name: 'app',
  components: {
    'todo-list': TodoList,
    'todo-edit-dialog': ToDoEditDialog
  },
  data: function() {
    return {
      keyword: "",
      todos: []
    };
  },
  methods: {
    openModalDialog(e, item) {
      const self = this;
      self.$refs.modalDialog.openDialog(e, item);
    },
    storeTodo(e, todo) {
      const self = this;
      if(todo.isCreate) {
          //新規
          self.todos.push(todo);
      } else {
        //変更
        const targetIndex = self.todos.findIndex(function(element, index, array){
          return element.id === todo.id;
        });
        if(targetIndex !== -1) {
          self.todos.splice(targetIndex, 1, todo);
        } else {
          self.todos.push(todo);
        }
      }
    },
    deleteTodo(e, todo) {
      const self = this;
      const targetIndex = self.todos.findIndex(function(element, index, array){
        return element.id === todo.id;
      });
      if(targetIndex !== -1) {
        self.todos.splice(targetIndex, 1);
      }
    }
  },
  computed: {
    filteredTodos() {
      const self = this;
      if(self.keyword === '') {
        return self.todos;
      } else {
        const keyword = self.keyword.split('　').join(' ').trim();
        if(keyword === '') {
          self.keyword = "";
          return self.filteredTodos();
        }
        const keywords = keyword.split(" ");
        const regexStr = keywords.map(function(element){
          return "(?=.*" + element + ")";
        }).join("");
        const regex = new RegExp("^" + regexStr);
        return self.todos.filter(function(element, index, array){
          return regex.test(element.context);
        });
      }
    }
  }
}
</script>

<style scoped>
  div.container {
    font-family: 'Avenir', Helvetica, Arial, sans-serif;
    -webkit-font-smoothing: antialiased;
    -moz-osx-font-smoothing: grayscale;
    text-align: center;
    color: #2c3e50;
    margin-top: 60px;
  }
  .todo-list-area {
    margin-top: 3rem;
  }

  p.save {
    position: fixed;
    top: 8%;
    right: 3%;
    z-index: 10;
  }

  p.save a {
    background: #31b0d5;
    color: #fff;
  }

  p.save a {
    opacity: .75;
    text-decoration: none;
    width: 50px;
    padding: 5px 0;
    text-align: center;
    display: block;
    border-radius: 5px;
    font-size: 200%;
  }
  p.save a:hover {
    background: #5bc0de;
  }
</style>
