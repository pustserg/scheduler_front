<template>
  <b-container fluid id="app">
    <b-row>
      <b-col></b-col>
      <b-col cols="8">
        <TasksList />
      </b-col>
      <b-col></b-col>
    </b-row>
  </b-container>
</template>

<script>
import TasksList from './components/TasksList.vue';

export default {
  name: 'app',
  components: {
    TasksList,
  },
};
</script>

<style lang="sass">
#app
  margin-top: 50px
</style>
