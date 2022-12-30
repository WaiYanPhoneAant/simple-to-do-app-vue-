<template>

  <div id="App" class="m-auto mt-lg-5" style="max-width: 650px;">
      <h1 class="text-center bg-secondary text-warning p-3 ">Ghost's To do list</h1>
      <div class="container">
        <div class="row my-4">
            <div class="col-8">
              <input type="text" class="form-control" placeholder="Enter your tasks" v-model="newTask" @keyup.enter="addText">
            </div>
            <div class="col-4">
              <button type="submit" class="btn btn-primary w-100" @click="addText">
                Add Tasks
              </button>
            </div>
        </div>
        <div class="row d-flex justify-content-end">
          <div class="col-5 " >
              <button type="submit" class="btn btn-danger w-100" @click="deleteTasks">
                Delete complete Tasks
              </button>
          </div>
        </div>
          <div class="row text-white">
            <div class="col-6">
               <h2 class="fw-light pt-2">Tasks</h2>
               
            </div>
            <div class="col-6">
                <h2 class="text-end  fw-light pt-2">Done</h2>
            </div>
          </div>
          <div class="row">
            <div class="p-2 alert alert-warning text-center mt-3" v-if="filterTask.length==0">
              There is no Task to Do 
            </div>
          </div>
          <div class="row mt-3" v-for="(task,index) in filterTask"  :key="task">
                <div class="col-6 text-white " :class="{'text-decoration-line-through':task.done}">{{task.actions}}</div>
                <div class="col-6 text-end"  >
                  <input type="checkbox" @change="cd(index)" name="" id="" v-model="task.done">
                </div>
          </div>
          <div class="row mt-3 p-3">
            <label  class="col-12 btn text-white" :class="hideComplete?'bg-primary':'bg-danger'" for="check">
              {{hideComplete?'Show complete tasks':'Hide complete taks'}}
            </label>
            <input type="checkbox" name="" v-model="hideComplete" id="check" style="display: none;">
          </div>
      </div>

  </div>
</template>

<script>
  export default{
    name:'App',
    data:()=>({
      name:"App",
      hideComplete:false,
      newTask:'',
      tasks:[],
    }),
    computed:{
      filterTask(){
        return this.hideComplete?this.tasks.filter((v)=>!v.done):this.tasks;
      }
    },
    methods: {
      hide(){
        let condition=this.hideComplete?0:1;
        localStorage.setItem('TasksCondition',JSON.stringify(condition));
        location.reload();
        this.hideComplete=this.hideComplete?false:true;
        
      },
      addText(){
        if(!this.newTask.trim()){
          return alert('Put Some Tasks')
        }else{
          this.tasks.push({actions:this.newTask,done:false})
          this.storeData()
          this.newTask="";

        }
      },
      cd(index){
        this.storeData()
        console.log(index);
      },
      deleteTasks(){
        this.tasks=this.tasks.filter((v)=>!v.done);
        this.storeData()
      },
      storeData(){
        localStorage.setItem('myLocalTasks',JSON.stringify(this.tasks));
      }
    },
    mounted() {
      let data=localStorage.getItem('myLocalTasks');
      let TasksCondition=localStorage.getItem('TasksCondition');
      if(data!=null){
        this.tasks=JSON.parse(data);
        
      }
      TasksCondition==0?this.hideComplete=false:this.hideComplete=true;
    },
  }
</script>

