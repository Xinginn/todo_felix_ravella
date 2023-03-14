<template>
  <div class="task-wrapper">
    <div>
      #{{ taskIndex +1 }} | {{ initialData.label }}
    </div>
    <div>
      Status: {{ initialData.status }}
    </div>
    <button type="button" @click="toggleEdit">{{isEditing ? 'Stop editing': 'Edit' }}</button>
    <div v-if="isEditing">
      <input v-model="mutableData.label"/>
    </div>

  </div>
</template>

<script>
export default {
  name: 'Task',
  props:{
    taskIndex: Number,
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
  ],
  methods: {
    toggleEdit(){
      this.isEditing = !this.isEditing;
      if(!this.isEditing){
        this.$emit('taskChanged', this.taskIndex, this.mutableData)
      }
    }
  }
}
</script>

<style scoped>
  .task-wrapper{
    border: solid black 1px;
  }
</style>