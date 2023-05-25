<script setup>
import { ref } from 'vue';
import { uid } from 'uid';
import JobPoster from '../components/JobPoster.vue';
const jobPosts = ref([]);
const createQuest = (quest) => {
  jobPosts.value.push({
    id:uid(),
    quest:,
    isCompleted: null,
    isEditing:null,
  });
};
</script>

<template>
  <main>
    <h1>Post Quest</h1>
    <JobPoster @create-quest="createQuest()"/>
  </main>
</template>
<style scoped lang="scss">
main {
  display: flex;
  flex-direction: column;
  max-width: 500px;
  width: 100%;
  margin: 0 auto;
  padding: 40px 16px;

  h1 {
    margin-bottom: 16px;
    text-align: center;
  }
}
</style>