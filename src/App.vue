<script setup>
import { ref } from "vue";

//New Todo
const newTodo = ref("");
const defaultData = [
  {
    done: false,
    content: "Einkaufen gehen",
  },
];

//Here we store all Todos
const todos = ref(JSON.parse(localStorage.getItem("todos")) || defaultData);

var addTodo = () => {
  if (newTodo.value) {
    todos.value.push({
      done: false,
      content: newTodo.value,
    });
    newTodo.value = "";
  }
  saveData();
};

var doneTodo = (todo) => {
  console.log(todo);
  todo.done = !todo.done;
  saveData();
};
function removeTodo(index) {
  todos.value.splice(index, 1);
  saveData();
}

var saveData = () => {
  const storageData = JSON.stringify(todos.value);
  localStorage.setItem("todos", storageData);
};
</script>

<template>
  <h1>ToDo App</h1>
  <form @submit.prevent="addTodo()">
    <label>Neue Aufgabe hinzufügen: </label>
    <input
      v-model="newTodo"
      placehodler="Enter your Todo here"
      name="newTodo"
      autocomplete="off"
    />
    <button :disabled="!newTodo">Aufgabe Hinzufügen</button>
  </form>
  <h2>Deine Aufgaben:</h2>
  <ul>
    <li v-for="(todo, index) in todos" :key="index">
      <span :class="{ done: todo.done }" @click="doneTodo(todo)">{{
        todo.content
      }}</span>
      <button @click="removeTodo(index)">Entfernen</button>
    </li>
  </ul>
  <h4 v-if="todos.length === 0">Deine Aufgabenliste ist leer 🤟</h4>
</template>

<style lang="scss">
:root {
  --border: 2px solid rgba(255, 255, 255, 0.35);
  --size1: 6px;
  --size2: 12px;
  --size3: 18px;
  --size4: 24px;
  --size5: 48px;
  --backgroundColor: #27292d;
  --textColor: white;
  --primaryColor: #a0a4d9;
  --secondTextColor: #1f2023;
}

body {
  margin: 0;
  padding: 0;
  font-family: Avenir, Helvetica, Arial, sans-serif;
  background-color: var(--backgroundColor);
  color: var(--textColor);
  #app {
    max-width: 600px;
    margin-left: auto;
    margin-right: auto;
    padding: 20px;
    h1 {
      font-weight: bold;
      font-size: 28px;
      text-align: center;
    }
    form {
      display: flex;
      flex-direction: column;
      width: 100%;
      label {
        font-size: 14px;
        font-weight: bold;
      }
      input,
      button {
        height: var(--size5);
        box-shadow: none;
        outline: none;
        padding-left: var(--size2);
        padding-right: var(--size2);
        border-radius: var(--size1);
        font-size: 18px;
        margin-top: var(--size1);
        margin-bottom: var(--size2);
      }
      button:disabled,
      button[disabled] {
        border: 1px solid #999999;
        background-color: #cccccc;
        color: #666666;
        cursor: not-allowed;
      }
      input {
        background-color: transparent;
        border: var(--border);
        color: inherit;
      }
      input::placeholder {
        font-weight: bold;
        opacity: 0.5;
        color: red;
      }
    }
    button {
      cursor: pointer;
      background-color: var(--primaryColor);
      border: 1px solid var(--primaryColor);
      color: var(--textColor);
      font-weight: bold;
      outline: none;
      border-radius: var(--size1);
    }
    h2 {
      font-size: 22px;
      border-bottom: var(--border);
      padding-bottom: var(--size1);
    }
    ul {
      padding: 10px;
      li {
        display: flex;
        justify-content: space-between;
        align-items: center;
        border: var(--border);
        padding: var(--size1) var(--size4);
        border-radius: var(--size1);
        margin-bottom: var(--size2);
        span {
          cursor: pointer;
        }
        .done {
          text-decoration: line-through;
        }
        button {
          font-size: var(--size2);
          padding: var(--size1);
        }
      }
    }
    h4 {
      text-align: center;
      opacity: 0.5;
      margin: 0;
    }
  }
}
</style>
