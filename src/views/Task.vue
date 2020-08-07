<template>
  <div
    class="task-view relative flex flex-row bg-white m32 mx-auto py-4 text-left mt-20 rounded shadow"
  >
    <div class="flex flex-col flex-grow items-start justify-between px-4">
      {{ task.name }}
    </div>
  </div>
</template>

<script>
import { mapGetters } from 'vuex'
export default {
  computed: {
    ...mapGetters(['getTask']),
    task() {
      return this.getTask(this.$route.params.id)
    },
  },
}
</script>

<style>
.task-view {
  max-width: 700px;
}
</style>
