<template>
  <div class="board">
    <div class="flex flex-row items-start">
      <div
        class="column bg-gray-400 p-2 mx-2 text-left shadow rounded mt-2"
        v-for="(column, i) of board.columns"
        :key="i"
      >
        <div class="flex items-center mb-2 font-bold text-gray-800">
          {{ column.name }}
        </div>
        <div class="list-none">
          <div
            class=" flex items-center flex-wrap shadow mb-2 py-2 rounded bg-white text-gray-800 no-underline"
            v-for="(task, i) of column.tasks"
            :key="i"
            @click="goToTask(task)"
          >
            <span class="w-full flex-shrink-0 font-bold ml-1">
              {{ task.name }}
            </span>
            <p
              class="w-full flex-shrink-0 mt-1 text-sm ml-1"
              v-if="task.description"
            >
              {{ task.description }}
            </p>
          </div>
        </div>
      </div>
    </div>
    <div class="task-bg h-screen" v-if="isTaskOpen" @click.self="close">
      <router-view />
    </div>
  </div>
</template>

<script>
import { mapState } from 'vuex'
export default {
  computed: {
    ...mapState(['board']),
    isTaskOpen() {
      return this.$route.name === 'task'
    },
  },
  methods: {
    goToTask(task) {
      this.$router.push({ name: 'task', params: { id: task.id } })
    },
    close() {
      this.$router.push({ name: 'board' })
    },
  },
}
</script>

<style lang="css">
.column {
  min-width: 350px;
}
.task-bg {
  position: absolute;
  top: 0;
  bottom: 0;
  left: 0;
  right: 0;
  background-color: rgba(0, 0, 0, 0.5);
}
</style>
