<template>
  <div class="tasks-list__item row" @mouseover="showButtons" @mouseleave="hideButtons">
    <div class="col-md-10">
      <h6>{{task.action}}</h6>
      {{task.schedule}} {{performAt}}
    </div>
    <div class="col-md-1" v-if="visibleButtons">
      <b-button variant="danger" @click="deleteTask()">
        <v-icon name="trash" />
      </b-button>
    </div>
    <div class="col-md-1" v-if="visibleButtons">
      <b-btn v-b-modal="'editTaskForm'" class="btn-success">
        <v-icon name="edit" />
      </b-btn>
    </div>
    <b-modal id="editTaskForm"
             hide-footer
             title="Edit task"
             size="lg"
             ref="editTaskModlRef">
      <TaskForm :closeModal="closeModal" :editTask="task" />
    </b-modal>
  </div>
</template>

<script>
import moment from 'moment';
import axios from 'axios';
import Icon from 'vue-awesome/components/Icon.vue';
import 'vue-awesome/icons/edit';
import 'vue-awesome/icons/trash';

import TaskForm from './TaskForm.vue';

export default {
  props: ['task', 'fetchTasks'],
  computed: {
    performAt() {
      const performAtTime = moment.unix(this.task.perform_at);
      return performAtTime.format('YYYY M D HH:mm:ss');
    },
  },
  data() {
    return {
      visibleButtons: false,
    };
  },
  methods: {
    deleteTask() {
      if (window.confirm('Are you sure?')) {
        axios.delete(`http://localhost:8000/tasks/${this.task.id}`)
          .then(() => this.fetchTasks());
      }
    },
    showButtons() {
      this.visibleButtons = true;
    },
    hideButtons() {
      this.visibleButtons = false;
    },
    closeModal() {
      this.fetchTasks();
      this.$refs.editTaskModlRef.hide();
    },
  },
  components: {
    TaskForm,
    'v-icon': Icon,
  },
};
</script>
<style lang="sass">
.tasks-list__item
  padding: 1em 0
</style>
