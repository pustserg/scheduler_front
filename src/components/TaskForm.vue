<template>
  <div class="task-form-wrapper">
    <b-form @submit="onSubmit">
      <b-form-group id="schedule-form-group"
                    label="Schedule"
                    label-for="schedule"
                    description="Cron format">
        <b-form-input id="schedule" v-model="task.schedule" required />
      </b-form-group>

      <b-form-group id="action-form-group"
                    label="Action"
                    label-for="action">
        <b-form-select id="action" v-model="task.action" :options="selectActions" required />
      </b-form-group>

      <b-form-group id="message-form-group"
                    label="Message"
                    label-for="message">
        <b-form-textarea id="message"
                         v-model.trim="task.message"
                         placeholder="message to send"
                         :rows="4">
        </b-form-textarea>
      </b-form-group>

      <b-button type="submit" variant="primary">Save</b-button>
    </b-form>
  </div>
</template>

<script>
import axios from 'axios';

export default{
  props: {
    editTask: Object,
    closeModal: Function,
  },
  data() {
    let task = this.editTask;
    if (task === undefined) {
      task = {
        schedule: '* * * * * * *',
        action: 'send_telegram_message',
        message: '',
      };
    } else {
      task = {
        schedule: this.editTask.schedule,
        action: this.editTask.action,
        message: '',
      };
    }
    return {
      task,
      selectActions: [
        { value: 'send_telegram_message', text: 'Отправить сообщение в телеграм' },
        { value: 'send_rabbit_message', text: 'Отправить сообщение в rabbit' },
      ],
    };
  },
  methods: {
    onSubmit(evt) {
      evt.preventDefault();
      if (this.editTask === undefined) {
        axios.post('http://localhost:8000/tasks', this.task)
          .then(() => this.closeModal());
      } else {
        axios.patch(`http://localhost:8000/tasks/${this.editTask.id}`, this.task)
          .then(() => this.closeModal());
      }
    },
  },
};
</script>

<style lang="sass">
</style>
