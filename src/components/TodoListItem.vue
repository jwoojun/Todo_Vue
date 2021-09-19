<template>
  <li>
    <span class="item" :class="todoItemClass" @click="toggleItem">{{
      item.title
    }}</span>
    <button @click="removeItem">삭제</button>
  </li>
</template>
<script lang="ts">
import { PropType } from "@vue/runtime-core/dist/runtime-core";
import Vue from "vue";
import { Todo } from "../App.vue";

export default Vue.extend({
  props: {
    item: Object as PropType<Todo>,
    index: Number,
  },
  methods: {
    removeItem() {
      // 모든 것을 vuex로 처리해서는 안된다.
      this.$emit("remove", this.index);
    },
    toggleItem() {
      this.$emit("toggle", this.item, this.index);
    },
  },
  computed: {
    todoItemClass(): string | null {
      return this.item.done ? "complete" : null;
    },
  },
});
</script>

<style scoped>
.item {
  cursor: pointer;
}
.complete {
  text-decoration: line-through;
}
</style>