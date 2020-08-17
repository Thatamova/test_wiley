<template>
  <div class="page">
    <Main :task-list="tasks" />
  </div>
</template>

<script>
import Main from '@/components/main/index';

export default {
  name: 'Home',

  components: {
    Main,
  },

  data() {
    return {
      // список актуальных задач
      tasks: [
        {
          title: 'Title 1', // заголовок задачи
          description: 'Lorem ipsum dolor sit amet, consectetur adipisicing elit.', // описание задачи
          isCompleted: false, // статус задачи
        },
        {
          title: 'Title 3',
          description: 'Lorem ipsum dolor sit amet, consectetur adipisicing elit.',
          isCompleted: false,
        },
        {
          title: 'Title 2',
          description: 'Lorem ipsum dolor sit amet, consectetur adipisicing elit.',
          isCompleted: false,
        },
        {
          title: 'Title 0',
          description: 'Lorem ipsum dolor sit amet, consectetur adipisicing elit.',
          isCompleted: false,
        },
      ],
    };
  },
};
</script>

<style scoped>
.page {
  width: 80%;
  padding: 20px;
  margin: 0 auto;
}
</style>
