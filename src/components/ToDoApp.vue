<template>
  <div class="container">
    <h2 class="text-center mt-5">My Vue To-Do App</h2>
<!--    Input-->
    <div class="d-flex">
      <input @keyup.enter="submitTask" v-model="task" type="text" placeholder="Enter Text" class="form-control">
      <button @click="submitTask" class="btn btn-warning rounded-0">Submit</button>
    </div>
    <p v-if="empty_validation" class="alert alert-danger mt-2 form-control p-1">{{ empty_validation }}
      <a  @click="cancelValidationMessage" ><i class="fa-solid fa-xmark"></i></a>
    </p>
    <!--      Task Table-->
    <table class="table table-bordered mt-3">
      <thead>
      <tr>
        <th scope="col">Task</th>
        <th scope="col">Status</th>
        <th scope="col" class="text-center">#</th>
        <th scope="col" class="text-center">#</th>
      </tr>
      </thead>
      <tbody>
      <tr v-for="(task, index) in tasks " :key="index">
        <td >
          <span :class="{'finished': task.status=='completed'} ">
            {{ task.name }}
          </span>
        </td>
        <td class="fixed_length" >
          <button
              :class="{
              'text-danger': task.status =='to-do',
              'text-warning': task.status =='in-progress',
              'text-success': task.status =='completed',
              }"
              @click="buttonWork(index)" >
            {{ task.status}}
          </button>
        </td>
        <td>
          <div class="text-center" @click="editTask(index)">
            <span class="fa fa-pen"></span>
          </div>
        </td>
        <td>
          <div class="text-center" @click="deleteTask(index)">
            <span class="fa fa-trash"></span>
          </div>
        </td>
      </tr>

      </tbody>
    </table>
  </div>
</template>

<script>
export default {
  name: 'ToDoApp',
  props: {
    msg: String
  },
  data() {
    return {

      task: '',
      editTaskIndex : null,
      status_array: ['to-do','in-progress','completed'],
      empty_validation: '',
      tasks: [
        {
          name: 'Steal banana form the store',
          status: 'to-do',
        },
        {
          name: 'Eat 1 kg chocolate',
          status: 'in-progress',
        },
      ]
    }
  },
  methods: {
    submitTask() {
      if( this.task.trim().length == 0){
        this.empty_validation ='Please enter you task name';
      }
      else{
        if( this.editTaskIndex == null){
          this.tasks.push({
            name: this.task,
            status: 'to-do',
          });
        }
        else{
          this.tasks[this.editTaskIndex].name = this.task;
          this.editTaskIndex = null;
        }
        this.task='';
        this.empty_validation='';
      }
    },
    deleteTask(index){
      this.tasks.splice(index,1);
    },
    cancelValidationMessage(){
      this.empty_validation=''
    },
    editTask(index){
      this.editTaskIndex = index;
      this.task = this.tasks[index].name;
    },
    buttonWork(index){
      let status_array_index = this.status_array.indexOf(this.tasks[index].status);
      if( ++status_array_index >2 ) status_array_index=0;
      this.tasks[index].status = this.status_array[status_array_index];
    }

  },

}
</script>

<!-- Add "scoped" attribute to limit CSS to this component only -->
<style scoped>
.fixed_length{
  width: 120px;
}
.finished{
  text-decoration: line-through;
}

</style>
