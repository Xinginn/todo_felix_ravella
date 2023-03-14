
<template>
  <h1>My TODO List</h1>

  New task:
  <form @submit.prevent="addTask">
    <input type="text" v-model="newTaskData.label" placeholder="task name">
    <select v-model="newTaskData.status">
      <option v-for="item, index of statusOptions" :hidden="item == statusOptions[statusOptions.length-1]"
        :key="index"
        :value="item">
        {{item}}
      </option>
    </select>
    <button type="submit">+</button>
  </form>

  <Task v-for="item,index of taskList" 
    :key="index" 
    :taskIndex="index"
    :initialData="item"
    @taskChanged="onTaskChanged">
  </Task>
</template>

<script>
import Task from './components/Task.vue'

export default {
  name: 'App',
  components: {
    Task
  },
  data(){
    return {
      taskList: [
        {
          label: 'première task',
          status: 'todo'
        },
        {
          label: 'deuxieme task',
          status: 'done'
        }
      ],
      newTaskData: {
        label: "",
        status: "todo"
      },
      statusOptions: [
        "todo",
        "doing...",
        "done!"
      ],
    }
  },
  methods:{
    onTaskChanged(index, data){
      this.taskList[index] = data
    },
    addTask(){
      this.taskList.push({...this.newTaskData})
      // flush new task values
      this.newTaskData = {
        label: "",
        status: "todo"
      }
    }
  }
}
</script>

<style scoped>
</style>
