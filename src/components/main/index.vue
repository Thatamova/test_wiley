<template>
  <div>
    <div v-if="taskListSorted && taskListSorted.length" class="task_list">
      <!-- выводим список задач -->
      <Task
        v-for="(task, index) in taskListSorted"
        :key="index"
        :task="task"
        :index-task="index"
        @editTask="onEditTask"
        @removeTask="onRemoveTask"
        @completeTask="onCompleteTask"
      >
      </Task>
    </div>

    <div v-else>
      Tasks list are empty
    </div>

    <!-- кнопка для добавления задач -->
    <Button
      icon="pi pi-plus"
      label="Add task"
      class="page__button"
      @click="showModalTask"
    />

    <!-- форма добавления задач -->
    <Dialog
      header="Add task"
      :visible.sync="isModalTask"
      :modal="true"
      :style="{width: '40%'}"
    >
      <FormTask @changeTask="onAddTask" />
    </Dialog>
  </div>
</template>

<script>
import Types from 'vue-types';
import Button from 'primevue/button';
import Dialog from 'primevue/dialog';
import Task from '@/components/task/index';
import FormTask from '@/components/form_task/index';

export default {
  name: 'Home',

  components: {
    Task,
    FormTask,
    Button,
    Dialog,
  },

  props: {
    taskList: Types.arrayOf(
      Types.shape({
        title: Types.string.def(''),
        description: Types.string.def(''),
        isCompleted: Types.bool.def(false),
      }).loose,
    ).def([]),
  },

  data() {
    return {
      isModalTask: false,
      taskListSorted: [],
    };
  },

  mounted() {
    if (localStorage.getItem('task_list')) {
      try {
        // если в localStorage хранится список задач, то преобразовываем его в массив
        this.taskListSorted = JSON.parse(localStorage.getItem('task_list'));
      } catch (e) {
        localStorage.removeItem('task_list');
      }
    } else {
      this.taskListSorted = this.getListSorted(this.taskList);
    }
  },

  watch: {
    taskListSorted() {
      // следим за изменениями в списке и обновляем localStorage
      this.saveTaskList();
    },
  },

  methods: {
    showModalTask() {
      this.isModalTask = true; // отображение модального окна для добавления задачи
    },

    // добавляем задачу в список
    onAddTask(task) {
      this.taskListSorted.push(task);
      this.isModalTask = false;
      this.getListSorted(this.taskListSorted);
    },

    // редактируем задачу
    onEditTask(index, task) {
      this.taskListSorted.splice(index, 1, task);
      this.getListSorted(this.taskListSorted);
    },

    // удаляем задачу
    onRemoveTask(index) {
      this.taskListSorted.splice(index, 1);
    },

    // задача выполнена
    onCompleteTask(index) {
      const completeTask = this.taskListSorted.find((task, key) => index === key);
      completeTask.isCompleted = true;

      this.taskListSorted.splice(index, 1, completeTask);
    },

    // сортируем список задач в обратном порядке по заголовку
    getListSorted(list) {
      return list.sort((a, b) => a.title.localeCompare(b.title)).slice().reverse();
    },

    // сохраняем задачи в localStorage
    saveTaskList() {
      const parsed = JSON.stringify(this.taskListSorted); // преобразуем массив в строку JSON
      localStorage.setItem('task_list', parsed);
    },
  },
};
</script>

<style scoped>
.page {
  width: 80%;
  padding: 20px;
  margin: 0 auto;
}

.task_list {
  width: 100%;
  display: flex;
  flex-wrap: wrap;
}

.page__button {
  margin-top: 60px;
}
</style>
