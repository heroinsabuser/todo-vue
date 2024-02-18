<script setup lang="ts">
import { onMounted, ref, type Ref } from "vue";

import MyTodo from "./components/MyTodo.vue";

interface ITodo {
  id: number;
  text: string;
  checked: boolean;
}

const todos: Ref<ITodo[]> = ref([]);

const text = ref("");

const updateStorage = () => {
  localStorage.setItem("todos", JSON.stringify(todos.value));
};

const addTodo = () => {
  if (text.value.length === 0) return;
  todos.value.push({
    id: Math.random(),
    text: text.value,
    checked: false,
  });
  text.value = "";
  updateStorage();
};

const removeTodo = (id: number) => {
  todos.value = todos.value.filter((todo) => todo.id !== id);
  updateStorage();
};

const checkTodo = (id: number) => {
  todos.value = todos.value.map((todo) => {
    if (todo.id === id) {
      todo.checked = !todo.checked;
    }
    return todo;
  });
  updateStorage();
};

onMounted(() => {
  todos.value = JSON.parse(localStorage.getItem("todos") || "[]");
});
</script>

<template>
  <div class="container">
    <div class="window">
      <div class="content">
        <div class="title">
          <h1>Todo App</h1>
          <div class="input-content">
            <input
              v-model="text"
              class="input"
              type="text"
              placeholder="Добавить задачу"
              @keyup.enter="addTodo"
              maxlength="32"
              minlength="1"
            />
            <img
              @click="addTodo"
              class="plus"
              v-if="text"
              src="/input-plus.svg"
              alt=""
            />
          </div>
        </div>
        <div class="todos" v-auto-animate>
          <MyTodo
            v-for="todo in todos"
            :key="todo.id"
            :text="todo.text"
            :checked="todo.checked"
            @onClickCheck="checkTodo(todo.id)"
            @onClickDelete="removeTodo(todo.id)"
          />
          <span v-if="todos.length === 0" class="empty">Список дел пуст</span>
        </div>
      </div>
    </div>
  </div>
</template>

<style scoped>
.empty {
  font-weight: normal;
  font-size: 14px;
  margin: 15px 0;
  text-align: center;
}
.plus {
  position: absolute;
  right: 0px;
  cursor: pointer;
  opacity: 0.5;
  transition: 0.5s;
}
.plus:hover {
  opacity: 1;
}
.input-content {
  position: relative;
  margin-bottom: 15px;
}
.input {
  padding-left: 10px;
  padding-right: 45px;
  height: 33px;
  width: 250px;
  outline: none;
  background-color: var(--soft-color);
  border: 1px solid var(--secondary-color);
  border-radius: 5px;
}
.todos {
  display: flex;
  flex-direction: column;
  gap: 30px;
  overflow-y: auto;
  max-height: 400px;
}
.container {
  display: flex;
  justify-content: center;
  background-color: var(--primary-color);
  min-height: 100dvh;
  max-height: 100%;
  width: 100vw;
}
.window {
  background-color: #fff;
  padding: 20px 50px 30px;
  margin: 50px 20px;
  min-width: 350px;
  width: 600px;
  border-radius: 20px;
  box-shadow: 10px 10px 5px rgba(0, 0, 0, 0.2);
  height: fit-content;
}
.content {
  display: flex;
  flex-direction: column;
  h1 {
    font-size: 40px;
    font-weight: normal;
    margin-bottom: 10px;
  }
}
.title {
  display: flex;
  justify-content: center;
  flex-direction: column;
  align-items: center;
  border-bottom: 2px solid var(--secondary-color);
  margin-bottom: 15px;
}
</style>
