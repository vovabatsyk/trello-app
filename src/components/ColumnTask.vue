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
  export default {
    name: 'ColumnTask',
    props: {
      task: {
        type: Object,
        required: true
      },
      taskIndex: {
        type: Number,
        required: true
      },
      columnIndex: {
        type: Number,
        required: true
      },
      column: {
        type: Object,
        required: true
      },
      board: {
        type: Object,
        required: true
      }
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
      moveTaskOrColumn(event, toTasks, toColumnIndex, toTaskIndex) {
        const type = event.dataTransfer.getData('type')
        if (type === 'task') {
          this.moveTask(
            event,
            toTasks,
            toTaskIndex !== undefined ? toTaskIndex : toTasks.length
          )
        } else {
          this.moveColumn(event, toColumnIndex)
        }
      },
      moveColumn(event, toColumnIndex) {
        const fromColumnIndex = event.dataTransfer.getData('from-column-index')

        this.$store.commit('MOVE_COLUMN', {
          fromColumnIndex,
          toColumnIndex
        })
      },
      moveTask(event, toTasks, toTaskIndex) {
        const fromColumnIndex = event.dataTransfer.getData('from-column-index')
        const fromTasks = this.board.columns[fromColumnIndex].tasks
        const fromTaskIndex = event.dataTransfer.getData('from-task-index')

        this.$store.commit('MOVE_TASK', {
          fromTasks,
          fromTaskIndex,
          toTasks,
          toTaskIndex
        })
      }
    }
  }
</script>

<style scoped>

</style>
