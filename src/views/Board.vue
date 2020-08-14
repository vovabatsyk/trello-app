<template>
    <div class="board">
        <div class="flex flex-row items-start">
            <BoardColumn
                    v-for="(column, $columnIndex) in board.columns"
                    :key="$columnIndex"
                    :column="column"
                    :columnIndex="$columnIndex"
                    :board="board"
            >
                >
            </BoardColumn
            >
            <div
                    class="column flex bg-gray-400 p-2 mx-2 text-left shadow rounded mt-2"
            >
                <input
                        type="text"
                        v-model="newColumnName"
                        class="mr-2 pl-2 flex-grow "
                        placeholder="New Column Name"
                        @keyup.enter="createColumn"
                />
            </div>
        </div>

        <div class="task-bg h-screen" v-if="isTaskOpen" @click.self="close">
            <router-view/>
        </div>
    </div>
</template>

<script>
  import {mapState} from 'vuex'
  import BoardColumn from '../components/BoardColumn'

  export default {
    data() {
      return {
        newColumnName: ''
      }
    },
    computed: {
      ...mapState(['board']),
      isTaskOpen() {
        return this.$route.name === 'task'
      }
    },
    methods: {
      close() {
        this.$router.push({name: 'board'})
      },

      createColumn() {
        this.$store.commit('CREATE_COLUMN', {
          name: this.newColumnName
        })

        this.newColumnName = ''
      }
    },
    components: {
      BoardColumn
    }
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
