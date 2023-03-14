<template>
  <div class="task-wrapper">
    <div :class="initialData.status === 'done!' ? 'done' : ''">
      #{{ taskId+1 }} | {{ initialData.label }}
    </div>
    <div :class="initialData.status === 'done!' ? 'done' : ''">
      Status: {{ initialData.status }}
    </div>
   
    <button type="button" @click="isEditing = !isEditing;">{{isEditing ? 'Cancel': 'Edit' }}</button>
    <TaskForm 
      v-if="isEditing === true"
      :isEdit="true"
      :initialData="initialData"
      @sendData="confirmChange">
    </TaskForm>
    <button type="button" @click="() => $emit('requireDelete',taskId)">X</button>
  </div>

</template>

<script>
import TaskForm from './TaskForm.vue'

export default {
  name: 'Task',
  components: {
    TaskForm
  },
  props:{
    taskId: Number,
    initialData: Object
  },
  mounted(){
    this.mutableData = {...this.initialData}
  },
  data(){
    return {
      isEditing: false,
      mutableData: {
        label: null,
        status: null
      }
    }
  },
  emits:[
    'taskChanged',
    'requireDelete'
  ],
  methods: {
    confirmChange(data){
      this.isEditing = false;
      this.$emit('taskChanged', this.taskId, {...data})
    }
  }
}
</script>

<style scoped>
  .task-wrapper{
    border: solid black 1px;
  }

  .done {
    text-decoration: line-through;
    color: gray;
  }
</style>