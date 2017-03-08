<template>
  <transition name="modal" v-if="showModal">
    <div class="modal-mask">
      <div class="modal-wrapper">
        <div class="modal-container">

          <div class="modal-header">
            <h3>ToDo{{label}}</h3>
          </div>

          <div class="modal-body">
            <input id="todo-modal-context" type="text" v-model="todoData.context" class="form-control" placeholder="ToDoを入力してください" @keypress.enter="storeTodo">
          </div>

          <div class="modal-footer">
            <button class="btn btn-default" @click="closeDialog">
              Close
            </button>
          </div>
        </div>
      </div>
    </div>
  </transition>
</template>

<script>

import DateFormat from 'dateformat'

export default {
  name: 'modal-dialog',
  data() {
    return {
      showModal: false,
      todoData: {
        context: "",
        id: null
      }
    };
  },
  methods:{
    openDialog(e, item) {
      const self = this;
      if(typeof item === 'undefined') {
        self.todoData.context = "";
        self.todoData.id = null;
      } else {
        self.todoData.context = item.context;
        self.todoData.id = item.id;
      }
      self.showModal = true;

      setTimeout(function(){
        document.getElementById("todo-modal-context").focus();
      }, 100);
    },
    closeDialog(e) {
      const self = this;
      self.showModal = false;
    },
    storeTodo(e) {
      const self = this;

      if(self.todoData.context === '') {
        return;
      }
      const isCreate = self.todoData.id === null;
      const todo = {
        context: self.todoData.context,
        lastUpdateDate: DateFormat(new Date(), "yyyy-mm-dd hh:MM:ss"),
        isCreate: isCreate,
        id: isCreate ? self.createId() : self.todoData.id
      };
      self.$emit('StoreTodo', e, todo);
      self.todoData.context = "";
      self.showModal = false;
    },
    createId() {
      return "id-" + new Date().getTime().toString(16)  + Math.floor(1000 * Math.random()).toString(16);
    }
  },
  computed:{
    label() {
      const self = this;
      return self.todoData.id === null ? "登録" : "変更"
    }
  }
}
</script>

<style scoped>
  .modal-mask {
    position: fixed;
    z-index: 9998;
    top: 0;
    left: 0;
    width: 100%;
    height: 100%;
    background-color: rgba(0, 0, 0, .5);
    display: table;
    transition: opacity .3s ease;
  }

  .modal-wrapper {
    display: table-cell;
    vertical-align: middle;
  }

  .modal-container {
    width: 300px;
    margin: 0px auto;
    padding: 20px 30px;
    background-color: #fff;
    border-radius: 2px;
    box-shadow: 0 2px 8px rgba(0, 0, 0, .33);
    transition: all .3s ease;
    font-family: Helvetica, Arial, sans-serif;
  }

  .modal-header h3 {
    margin-top: 0;
    color: #42b983;
  }

  .modal-body {
    margin: 20px 0;
  }

  .modal-default-button {
    float: right;
  }

  .modal-enter {
    opacity: 0;
  }

  .modal-leave-active {
    opacity: 0;
  }

  .modal-enter .modal-container,
  .modal-leave-active .modal-container {
    -webkit-transform: scale(1.1);
    transform: scale(1.1);
  }
</style>
