<template>
  <div class="task">
    <Card :class="classes">
      <template slot="title">
        {{ task.title }}
      </template>
      <template slot="content">
        {{ task.description }}
      </template>
      <template slot="footer">
        <Button
          v-if="!task.isCompleted"
          icon="pi pi-check"
          label="Complete"
          class="p-button-success"
          @click="completeTask"
        />

        <Button
          v-if="!task.isCompleted"
          icon="pi pi-pencil"
          label="Edit"
          @click="showModalEditTask"
        />
        <Button
          icon="pi pi-times"
          label="Delete"
          class="p-button-secondary task__button task__button_del"
          @click="removeTask"
        />
      </template>
    </Card>

    <!-- форма редактирования задач -->
    <Dialog
      header="Edit task"
      :visible.sync="isModalTask"
      :modal="true"
      :style="{width: '40%'}"
    >
      <FormTask
        :task="task"
        @changeTask="onEditTask"
      />
    </Dialog>
  </div>
</template>

<script>
import Types from 'vue-types';
import Card from 'primevue/card';
import Button from 'primevue/button';
import Dialog from 'primevue/dialog';
import FormTask from '@/components/form_task/index';

export default {
  name: 'Task',

  components: {
    Card,
    Button,
    Dialog,
    FormTask,
  },

  props: {
    task: Types.shape({
      title: Types.string.def(''),
      description: Types.string.def(''),
      isCompleted: Types.bool.def(false),
    }).loose,
    indexTask: Types.number.def(0),
  },

  data() {
    return {
      isModalTask: false,
    };
  },

  computed: {
    classes() {
      return [
        'task__wrapper',
        // если задача выполенна, меняем подсветку на серую
        this.task.isCompleted ? 'task__wrapper_completed' : '',
      ];
    },
  },

  methods: {
    showModalEditTask() {
      this.isModalTask = true;
    },

    onEditTask(task) {
      this.$emit('editTask', this.indexTask, task);
      this.isModalTask = false;
    },

    removeTask() {
      this.$emit('removeTask', this.indexTask);
    },

    completeTask() {
      this.$emit('completeTask', this.indexTask);
    },
  },
};
</script>

<style scoped>
.task {
  width: 30%;
  margin: 12px;
}

.task__wrapper.task__wrapper_completed {
  background: #eee;
}

.task__button.task__button_del {
  margin-left: 20px;
}
</style>
