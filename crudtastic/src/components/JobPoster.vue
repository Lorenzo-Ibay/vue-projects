<script setup>
import { reactive, ref } from 'vue';

const quest = ref("");

const questState = reactive({
    quest: "Add quests here",
});
</script>

<template>
    <div class="input-wrap">
        <input v-model="questState.quest" type="text" />
        <button>Post</button>
    </div>
    <p> {{ questState.quest }}</p>
</template>

<style lang="scss" scoped>
.input-wrap {
  display: flex;
  transition: 250ms ease;
  border: 2px solid #41b080;

  &:focus-within {
    box-shadow: 0 -4px 6px -1px rgb(0 0 0 / 0.1),
      0 -2px 4px -2px rgb(0 0 0 / 0.1);
  }

  input {
    width: 100%;
    padding: 8px 6px;
    border: none;

    &:focus {
      outline: none;
    }
  }

  button {
    padding: 8px 16px;
    border: none;
  }
}
</style>