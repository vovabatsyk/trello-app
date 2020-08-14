<template>
    <div>
        <div
                class=" flex items-center flex-wrap shadow mb-2 py-2 rounded bg-white text-gray-800 no-underline"

                draggable
                @dragstart="pickupTask($event, taskIndex, columnIndex)"
                @click="goToTask(task)"
                @dragover.prevent
                @dragenter.prevent
                @drop.stop="moveTaskOrColumn($event, column.tasks, columnIndex, taskIndex)"
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
</template>

<script>
    import movingTasksAndColumnsMixin from '../mixins/movingTasksAndColumnsMixin'
  export default {
    name: 'ColumnTask',
    mixins: [movingTasksAndColumnsMixin],
    props: {
      task: {
        type: Object,
        required: true
      },
      taskIndex: {
        type: Number,
        required: true
      },
    },
    methods: {
      goToTask(task) {
        this.$router.push({name: 'task', params: {id: task.id}})
      },
      pickupTask(event, taskIndex, fromColumnIndex) {
        event.dataTransfer.effectAllowed = 'move'
        event.dataTransfer.dropEffect = 'move'

        event.dataTransfer.setData('from-task-index', taskIndex)
        event.dataTransfer.setData('from-column-index', fromColumnIndex)
        event.dataTransfer.setData('type', 'task')
      },
    }
  }
</script>

<style scoped>

</style>
