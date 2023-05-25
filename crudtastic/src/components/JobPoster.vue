<script setup>
import { reactive, ref } from 'vue';

const emit = defineEmits(["create-quest"]);
const questState = reactive({
  quest: "",
  invalid: false,
  errMsg:"",
});

const createQuest = () => {
  quest.invalid= false;
  if(quest.quest !==""){
    emit ("create-quest" , quest.quest);
    quest.quest = "";
    return;
  }
  quest.invalid = true;
  quest.errMsg = "Post Value Cannot Be Empty!";
};
</script>

<template>
    <div class="input-wrap">
        <input v-model="questState.quest" type="text" />
        <button @click="createQuest()">Post</button>
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