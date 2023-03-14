<template>
  <form @submit.prevent="submit">
    <input type="text" v-model="mutableData.label" placeholder="task name">
    <select v-model="mutableData.status">
      <option v-for="item, index of statusOptions" 
        :selected="item === statusOptions[0]"
        :hidden="isEdit === false && item === statusOptions[statusOptions.length-1]"
        :key="index"
        :value="item">
        {{item}}
      </option>
    </select>
    <button :disabled="mutableData.label === ''">{{isEdit ?'Confirm' : 'Add'}}</button>
  </form>
</template>

<script>
export default {
  name: 'TaskForm',
  props: {
    initialData: Object,
    isEdit: Boolean
  },
  mounted(){
    if (this.isEdit){
      this.mutableData = {...this.initialData}
    }
  },
  data(){
    return {
      mutableData: {
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
    submit(){
      this.$emit('sendData', this.mutableData);
      this.mutableData = {
        label: "",
        status: "todo"
      };
    }
  }
}
</script>