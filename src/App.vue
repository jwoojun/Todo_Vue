<template>
  <div>
    <header>
      <h1>Vue Todo with TS</h1>
    </header>
    <main>
      <TodoInput
        :item="item"
        @input="updateTodoText"
        @add="addTodoItem"
      ></TodoInput>
      <div>
        <ul>
          <TodoListItem
            v-for="(value, key) in todoItems"
            :key="key"
            :item="value"
            @remove="removeTodo"
            :index="key"
            @toggle="toggleTodoItemComplete"
          />
        </ul>
      </div>
    </main>
  </div>
</template>

<script lang="ts">
import Vue from "vue";
import TodoInput from "./components/TodoInput.vue";
import TodoListItem from "./components/TodoListItem.vue";

const STORAGE_KEY = "vue-todo-ts-v1";
const storage = {
  save(todoItems: Todo[]) {
    const parsed = JSON.stringify(todoItems);
    localStorage.setItem(STORAGE_KEY, parsed);
  },
  fetch(): Todo[] {
    const todoItems = localStorage.getItem(STORAGE_KEY) || "[]";
    const result = JSON.parse(todoItems);
    // 반환 타입: any
    return result;
  },
};

export interface Todo {
  title: string;
  done: boolean;
}

export default Vue.extend({
  name: "App",
  data() {
    return {
      item: "",
      todoItems: [] as Todo[],
    };
  },
  methods: {
    toggleTodoItemComplete(item: Todo, index: number) {
      this.todoItems.splice(index, 1, {
        ...item,
        done: !item.done,
      });
      storage.save(this.todoItems);
    },
    updateTodoText(value: string) {
      this.item = value;
    },
    addTodoItem() {
      const value = this.item;
      // localStorage.setItem(value, value);
      const todo: Todo = {
        title: value,
        done: false,
      };
      this.todoItems.push(todo);
      storage.save(this.todoItems);
      this.initTodoText();
    },
    initTodoText() {
      this.item = "";
    },
    fetchTodoItems() {
      this.todoItems = storage.fetch().sort((a, b) => {
        if (a.title < b.title) {
          return -1;
        }
        if (a.title > b.title) {
          return 1;
        }
        return 0;
      });
    },
    removeTodo(index: number) {
      console.log("remove", index);
      this.todoItems.splice(index, 1);
      storage.save(this.todoItems);
    },
  },
  created() {
    this.fetchTodoItems();
  },
  components: { TodoInput, TodoListItem },
});
</script>

<style>
</style>
