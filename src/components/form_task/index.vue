<template>
  <form
    class="form"
    novalidate
    @submit.prevent="sendForm"
  >
    <InputText
      v-model="modelForm.title"
      type="text"
      placeholder="Title"
      maxLength="100"
      class="form__field"
    />

    <Textarea
      v-model="modelForm.description"
      rows="5"
      maxLength="3000"
      placeholder="Description"
      :autoResize="false"
      class="form__field"
    />

    <!-- возможная доработка: полная валиадация формы с отображением ошибок валидации -->
    <Button
      v-if="!task"
      class="p-button-l form__button"
      label="Add"
      type="submit"
      :disabled="disabledButton"
    />

    <Button
      v-else
      class="p-button-l form__button"
      label="Edit"
      type="submit"
    />
  </form>
</template>

<script>
import Types from 'vue-types';
import InputText from 'primevue/inputtext';
import Textarea from 'primevue/textarea';
import Button from 'primevue/button';

export default {
  name: 'Task',

  components: {
    Button,
    InputText,
    Textarea,
  },

  props: {
    task: Types.shape({
      title: Types.string.def(''),
      description: Types.string.def(''),
      isCompleted: Types.bool.def(false),
    }).loose,
  },

  data() {
    return {
      modelForm: {
        title: this.task ? this.task.title : '',
        description: this.task ? this.task.description : '',
      },
    };
  },

  computed: {
    // блокируем кнопку если форма пустая
    disabledButton() {
      return this.modelForm.title.length < 1 || this.modelForm.title.length > 100
        || this.modelForm.description.length > 3000;
    },
  },

  methods: {
    sendForm() {
      const task = {
        ...this.modelForm,
        ...(!this.task ? { isCompleted: false } : { isCompleted: this.task.isCompleted }),
      };
      this.$emit('changeTask', task);
    },
  },
};
</script>

<style scoped>
.form {
  display: flex;
  flex-direction: column;
}

.form__field {
  width: 100%;
  margin-top: 16px;
}

.form__button {
  width: 20%;
  margin-top: 16px;
  align-self: center;
}
</style>
