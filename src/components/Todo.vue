<template>
  <div>
    <h1>ToDoリスト</h1>
    <input type="radio"  name="status" value="all" v-model="selectedStatus">全て
    <input type="radio" name="status" value="working" v-model="selectedStatus">作業中
    <input type="radio" name="status" value="complete" v-model="selectedStatus">完了
    <table>
      <tr>
        <th>ID</th>
        <th>タスク</th>
        <th>状態</th>
      </tr>
      <tr v-for="(task, index) in filteredTasks" v-bind:key="index" >
        <td v-text="task.id"></td>
        <td v-text="task.title"></td>
        <td>
          <button v-text="task.status" v-on:click="changeStatus(task.id)"></button>
          <button v-on:click="deleteTask(task.id)" >削除</button>
        </td>
      </tr>
    </table>
    <input v-model="taskTitle" placeholder="タスクを入力">
    <button v-on:click="addTask(taskTitle)">追加</button>
  </div>
</template>

<script>
export default {
  name: 'TodoApp',
  data () {
    return {
      tasks: [],
      taskTitle: '',
      selectedStatus: ''
    }
  },
  methods: {
    // タスクの削除
    deleteTask: function (id) {
      const tasks = this.tasks
      tasks.splice(id - 1, 1)
      const taskLength = tasks.length
      for (let i = id - 1; i < taskLength; i++) {
        this.$set(this.tasks[i], 'id', i + 1)
      }
    },
    // タスクの状態の変更
    changeStatus: function (id) {
      this.$set(this.tasks[id - 1], 'status', '完了')
    },
    // タスクの追加
    addTask: function (taskTitle) {
      if (taskTitle) {
        const tasks = this.tasks
        const taskNew = {id: tasks.length + 1, title: taskTitle, status: '作業中'}
        tasks.push(taskNew)
        this.taskTitle = ''
      }
    },
    // フィルター後のタスク一覧を取得
    getFilteredTasks: function (status, tasks) {
      switch (status) {
        case 'working':
          return tasks.filter(x => x.status === '作業中')
        case 'complete':
          return tasks.filter(x => x.status === '完了')
        default:
          return tasks
      }
    }
  },
  computed: {
    // フィルター後のタスク一覧
    filteredTasks: function () {
      const selectedStatus = this.selectedStatus
      const tasks = this.tasks
      return this.getFilteredTasks(selectedStatus, tasks)
    }
  }
}
</script>

<!-- Add "scoped" attribute to limit CSS to this component only -->
<style scoped>
</style>