<template>
  <w-app>
    <div class="pa12 ma6">
      <w-card title="simple todo list" title-class="blue-light5--bg">
        <w-form @submit.prevent="addTodo" class="mb2">
          <w-flex>
            <w-input
              class=".justify-start xs12"
              required
              label="enter a new todo item"
              v-model="newTodo"
              name="newTodo"
            />
            <div class="spacer"></div>
            <w-button
              @click="addTodo"
              class=".justify-end  ma1"
              bg-color="success"
              icon="wi-check"
              :disabled="!newTodo.length > 0"
            ></w-button>
          </w-flex>
        </w-form>
        <ul v-for="(todo, index) in todoItems" :key="todo.id" class="todo">
          <li class="pa2">
            <w-button
              @click="deleteTodo(index)"
              class="mr2"
              bg-color="error"
              icon="wi-cross"
            ></w-button>
            <span :class="{ done: todo.done }" @click="toggleTodo(todo)">
              {{ todo.task }}
            </span>
          </li>
        </ul>
      </w-card>
      <w-flex justify-end>
        <p @click="showDialog = true" class="about caption pt6">About</p>
      </w-flex>
      <w-dialog
        v-model="showDialog"
        :fullscreen="false"
        width="450"
        title-class="primary-light1--bg white"
      >
        <template #title>
          <w-icon class="pr3">mdi mdi-information</w-icon>
          About
        </template>
        <div class="lh4">
          <p class="title3">Welcome to the simple todo list.</p>
          <p class="body">
            This application allows to to add, mark complete and delete todo
            items. Items are stored in the browser's local storage and are
            therefore kepts between browser sessions.
          </p>
          <p class="body">
            Todo items are not stored on the server, nor are any logs of
            activity stored on the server.
          </p>
          <hr />

          <p class="body text-center">
            Made with
            <w-icon class="pr3 pl3 pb2" color="error">mdi mdi-heart</w-icon> in
            the UK using VueJs 3.
          </p>
          <p class="body text-center">
            Download the source
            <a href="https://github.com/doobrie/simple-todo-list">here</a>
          </p>
          <p class="body text-center">Copyright(c) David Salter 2021</p>
          <p class="body text-right">version 1.0.1</p>
        </div>
      </w-dialog>
    </div>
  </w-app>
</template>

<script>
import { ref } from 'vue';
import { onMounted } from 'vue';
import { v4 as uuidv4 } from 'uuid';
import '@mdi/font/css/materialdesignicons.min.css';

export default {
  setup() {
    onMounted(() => {
      if (window.localStorage.getItem('todoList')) {
        todoItems.value = JSON.parse(window.localStorage.getItem('todoList'));
      }
    });

    const newTodo = ref('');
    const todoItems = ref([]);
    const showDialog = ref(false);

    function addTodo() {
      todoItems.value.push({
        id: uuidv4(),
        done: false,
        task: newTodo.value,
      });
      newTodo.value = '';

      persistItems();
    }

    function toggleTodo(todo) {
      todo.done = !todo.done;
      persistItems();
    }

    function deleteTodo(index) {
      todoItems.value.splice(index, 1);
      persistItems();
    }

    function persistItems() {
      window.localStorage.setItem('todoList', JSON.stringify(todoItems.value));
    }

    return {
      addTodo,
      deleteTodo,
      newTodo,
      showDialog,
      todoItems,
      toggleTodo,
    };
  },
};
</script>

<style>
* {
  font-family: 'Roboto';
}

li {
  list-style-type: none;
}

.todo {
  cursor: pointer;
}

.about {
  cursor: pointer;
}

.done {
  text-decoration: line-through;
}
</style>
