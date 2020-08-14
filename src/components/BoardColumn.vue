<template>
    <div
            class="column bg-gray-400 p-2 mx-2 text-left shadow rounded mt-2"
            draggable
            @drop="moveTaskOrColumn($event, column.tasks, columnIndex)"
            @dragover.prevent
            @dragenter.prevent
            @dragstart.self="pickupColumn($event, columnIndex)"
    >
        <div class="flex items-center mb-2 font-bold text-gray-800">
            {{ column.name }}
        </div>
        <div class="list-decimal">
            <column-task
                    v-for="(task, $taskIndex) of column.tasks"
                    :key="$taskIndex"
                    :task="task"
                    :taskIndex="$taskIndex"
                    :column="column"
                    :columnIndex="columnIndex"
                    :board="board"
            />
            <input
                    type="text"
                    class="block p-2 w-full bg-transparent"
                    placeholder="+ Enter new task"
                    @keyup.enter="createTask($event, column.tasks)"
            />
        </div>
    </div>
</template>

<script>
  import ColumnTask from './ColumnTask'
  import movingTasksAndColumnsMixin from '../mixins/movingTasksAndColumnsMixin'

  export default {
    mixins: [movingTasksAndColumnsMixin],
    methods: {

      createTask(event, tasks) {
        this.$store.commit('CREATE_TASK', {
          tasks,
          name: event.target.value
        })
        event.target.value = ''
      },
      pickupColumn(event, fromColumnIndex) {
        event.dataTransfer.effectAllowed = 'move'
        event.dataTransfer.dropEffect = 'move'

        event.dataTransfer.setData('from-column-index', fromColumnIndex)
        event.dataTransfer.setData('type', 'column')
      }
    },
    components: {ColumnTask}
  }
</script>

<style lang="scss" scoped></style>
