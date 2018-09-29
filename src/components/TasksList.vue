<template>
  <div class="tasks-list">
    <h3 class="text-center">
      Tasks list
    </h3>
    <div class="add-task">
      <b-btn v-b-modal="'addTaskForm'" class="btn-success">Add task</b-btn>
    </div>
    <b-modal id="addTaskForm"
             hide-footer
             title="Add task"
             size="lg"
             ref="addTaskModalRef">
      <TaskForm :closeModal="closeModal" />
    </b-modal>
    <Task v-for="task in tasks"
          :task="task"
          :fetchTasks="fetchTasks"
          :key="task.id" />
  </div>
</template>

<script>
import axios from 'axios';
import Task from './Task.vue';
import TaskForm from './TaskForm.vue';

export default {
  components: {
    Task,
    TaskForm,
  },
  data() {
    return {
      loaded: false,
      tasks: [],
    };
  },
  mounted() {
    this.fetchTasks();
  },
  methods: {
    fetchTasks() {
      axios.get('http://localhost:8000/tasks')
        .then((response) => {
          this.loaded = true;
          this.tasks = response.data;
        });
    },
    closeModal() {
      this.fetchTasks();
      this.$refs.addTaskModalRef.hide();
    },
  },
};
</script>
<style lang="sass">
</style>
