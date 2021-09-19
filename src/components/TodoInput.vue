<template>
  <div>
    <label for="todo-input">오늘 할 일: </label>
    <input id="todo-input" type="text" :value="item" @input="handleInput"/>
    <button @click="addTodo" type="button">추가</button>
  </div>
</template>
<script lang="ts">
import Vue from "vue";
export default Vue.extend({
    props:{
        item:{
            type: String,
            required: true,
        }
    },
  methods: {
      // TS에서 기본 타입을 설정하지 않으면 모두 any로 처리한다.
      handleInput(event: InputEvent){
            const eventTarget = event.target as HTMLInputElement
            // event.target.value;
            this.$emit("input", eventTarget.value);
      },
    addTodo() {
        this.$emit("add")
    },
  },
});
</script>