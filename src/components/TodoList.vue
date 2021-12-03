<script>
import { ref } from 'vue';
import { TransitionRoot, TransitionChild } from "@headlessui/vue";
// ok it works
export default{

  data(){
    return{
      inputedTask: '',
      editTaskList: null,
      availableStatuses: ['Open', 'In Progress', 'Completed'],
      tasks: []
    }
  },

  methods:{
    submitTask(){
      console.log(this.inputedTask)
      if (this.inputedTask.length === 0) return

      if(this.editTaskList === null){
          this.tasks.push({
          name: this.inputedTask,
          status: 'Open'
        })
      }else{
        this.tasks[this.editTaskList].name = this.inputedTask
        this.editTaskList = null
      }
      

      this.inputedTask = ''
    },

    deleteTask(index){
      this.tasks.splice(index, 1)
    },

    editTask(index){
      this.inputedTask = this.tasks[index].name
      this.editTaskList = index
    },

    changeStatus(index){
      let newIndex = this.availableStatuses.indexOf(this.tasks[index].status)
      if(++newIndex > 2){
        newIndex =0
      } 
      this.tasks[index].status = this.availableStatuses[newIndex]
    }
  }


}

</script>

<template>
  
    <div class=" big-div">
        <div class="mid-div ">
             <div class=" mx-auto">
                 <h1>My Todo List</h1>
             </div>
             <div class="form-div">
                 <form @submit.prevent="submitTask">
                     <input v-model="inputedTask" class="" type="text" placeholder="Add Task" >
                     <button class="">ADD TASK</button>
                 </form>
             </div>

              <div class=" task-card">
                  <h2 v-if="tasks.length === 0" class=" px-1  flex-1  mt-4">No task at the moment. You can add task to work on</h2>
                  <h2 v-else class=" px-1  flex-1  mt-4 ">You have {{tasks.length}} tasks that needs attention</h2>
                    <div class=" todo-wrapper"> 
                        <div v-for="(task, index) in tasks" :key="index" class="todo-card">
                            <div class=" todolistmain ">{{task.name}} <span v-if="task.status === 'Completed' " class="finish-indicator"></span> </div>
                            <div @click=" changeStatus(index)"  :class=" { 'text-red-500': task.status === 'Open', 'text-yellow-500': task.status === 'In Progress', 'text-blue-500': task.status === 'Completed' }" class=" status-card">{{task.status}}</div>
                            <div @click="editTask(index)" class="edit">
                                <svg xmlns="http://www.w3.org/2000/svg" class="h-6 w-6 hover:text-green-800 text-green-700" fill="none" viewBox="0 0 24 24" stroke="currentColor">
                                    <path stroke-linecap="round" stroke-linejoin="round" stroke-width="2" d="M15.232 5.232l3.536 3.536m-2.036-5.036a2.5 2.5 0 113.536 3.536L6.5 21.036H3v-3.572L16.732 3.732z" />
                                </svg>
                            </div>
                            <div @click="deleteTask(index)" class=" edit">
                                <svg xmlns="http://www.w3.org/2000/svg" class="h-6 w-6 hover:text-red-500 text-red-400" fill="none" viewBox="0 0 24 24" stroke="currentColor">
                                    <path stroke-linecap="round" stroke-linejoin="round" stroke-width="2" d="M19 7l-.867 12.142A2 2 0 0116.138 21H7.862a2 2 0 01-1.995-1.858L5 7m5 4v6m4-6v6m1-10V4a1 1 0 00-1-1h-4a1 1 0 00-1 1v3M4 7h16" />
                                </svg>
                            </div>       
                        </div>
                    </div> 
             </div>

        </div>
    </div>

</template>

<style scoped>

.big-div{
  @apply text-gray-300 font-asap min-h-screen bg-todo-bg flex justify-center 
}
.mid-div{
  @apply w-4/5 md:w-4/6 lg:w-4/12 py-20 flex flex-col 
}
h1{
  @apply text-green-700 text-4xl font-semibold
}
.form-div{
  @apply  mx-10 sm:mx-10 flex flex-col mt-8 border-1  border-green-700 
}
form{
  @apply flex flex-col  flex-1 md:flex-row
}
input{
  @apply py-2  cursor-auto px-2 outline-none bg-todo-bg text-gray-500 font-light text-xl flex-1 placeholder-gray-500
}
button{
  @apply py-2  px-3 bg-green-700 font-medium  hover:bg-green-600
}
.task-card{
  @apply flex flex-col px-10
}
.todo-wrapper{
  @apply flex flex-col-reverse
}
.todo-card{
  @apply flex mt-2 border-1 border-green-700 divide-x-1 divide-green-700 
}
.todolistmain{
  @apply  p-2 flex flex-1 relative 
}
.status-card{
  @apply p-2 cursor-pointer flex justify-center  w-24 
}
.edit{
  @apply p-2 cursor-pointer w-10 flex-shrink-0
}
.finish-indicator{
  @apply flex border-b-1 border-yellow-300 w-4/5 absolute top-1/2 -left-0
}
.boo-enter-from{
  opacity: 0;
}
.boo-enter-to{
  opacity: 1;
}
.boo-enter-active{
  transition: all 2s ease-in
}
.boo-leave-from{
  opacity: 1;
}
.boo-leave-to{
   opacity: 0;
}
.boo-leave-active{
 transition: all 2s ease-out
}

</style>
