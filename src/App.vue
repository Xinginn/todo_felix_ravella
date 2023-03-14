
<template>
  <h1>My TODO List</h1>
  Filter tasks:
  <select v-model="selectedFilter">
    <option v-for="item,index of filterOptions" 
      :key="index" 
      :value="item"
    >
    {{item}}
    </option>
  </select>
  <hr>
  New task:
  <TaskForm 
    :isEdit="false"
    @sendData="addTask">
  </TaskForm>

  <Task v-for="item,index of getFilteredTasks" 
    :key="index" 
    :taskId="item.id"
    :initialData="item.data"
    @taskChanged="onTaskChanged"
    @requireDelete="onRequireDelete">
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
          id: 0,
          data: {
            label: 'First task',
            status: 'todo'
          }
        },
        {
          id: 1,
          data: {
            label: 'Second task',
            status: 'done!'
          }
        }
      ],
      selectedFilter:'all',
      filterOptions: [
        "all",
        "todo",
        "doing...",
        "done!"
      ],
      lastId: 1
    }
  },
  computed:{
    getFilteredTasks(){
      const filtered = this.tasks.filter((item) => {
        return (this.selectedFilter === 'all' || item.data.status === this.selectedFilter)
      })
      return filtered
    }
  },
  methods:{
    removeAllDone(){
      this.tasks = this.tasks.filter((item) => {
        return item.data.status !== 'done!'
      })
    },
    addTask(formData){
      this.lastId += 1;
      this.tasks.push(
        {
          id: this.lastId,
          data: {...formData}
         }
      )
    },
    onTaskChanged(taskId, data){
      for(let task of this.tasks){
        if (task.id === taskId){
          task.data = data;
          return
        }
      }
    },
    onRequireDelete(taskId){
      for(let index in this.tasks){
        if (this.tasks[index].id === taskId){
          this.tasks.splice(index,1);
          return
        }
      }
    }
  }
}
</script>

<style scoped>
</style>
