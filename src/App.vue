
<template>
  <h1>My TODO List</h1>
  New task:
  <TaskForm 
    :isEdit="false"
    @sendData="(data) => tasks.push({...data})">
  </TaskForm>

  <Task v-for="item,index of tasks" 
    :key="index" 
    :taskIndex="index"
    :initialData="item"
    @taskChanged="(index, data) => tasks[index] = data"
    @requireDelete="(index) => tasks.splice(index,1)">
  </Task>

  <button type="button" @click="removeAllDone">Clear all finished tasks</button>
</template>

<script>
import Task from './components/Task.vue'
import TaskForm from './components/TaskForm.vue'

export default {
  name: 'App',
  components: {
    Task,
    TaskForm
  },
  data(){
    return {
      tasks: [
        {
          label: 'première task',
          status: 'todo'
        },
        {
          label: 'deuxieme task',
          status: 'done!'
        }
      ],
    }
  },
  methods:{
    removeAllDone(){
      this.tasks = this.tasks.filter((item) => {
        console.log(item.status !== 'done!')
        return item.status !== 'done!'
      })
    }
  }
}
</script>

<style scoped>
</style>
