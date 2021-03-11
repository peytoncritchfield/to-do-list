<template>
  <div id="app">
    
    
    <div id="createNewTask">
      <div class="column">
      Create New Task
      </div>
        <div class="column">
          <div class="button bg-orange" @click="isActive = !isActive">+</div>
        </div>
    </div>

    <div v-if="isActive" :key="0">
      <input id="newTaskInput" v-model="newTask.name"/>

      <div v-if="hasMessage">{{message}}</div>

      <button id="submitNewTask" @click="addTask(newTask)">add task</button>
    </div>
    <p>
      Task
    </p>

      
        <template v-for="task in db.tasks">
          <div v-if="!task.isCompleted" class="task" :key="`pending-${task.id}`" >
            <div class="column"> {{task.name}} </div>
            <div class="column"> 
              <div class="button bg-white" @click="task.isCompleted = true"> 
                
               </div> 
            </div>
          </div>
          
        </template>
      
    <p>
      Completed
    </p>

    <template v-for="task in db.tasks">
      <div v-if="task.isCompleted" class="task" :key="`completed-${task.id}`">
      <div class="column"> {{task.name}} </div>
      <div class="column"> 
        <div class="button bg-orange" @click="task.isCompleted = false"> 
          <img src="./assets/check.png">
        </div>
      </div>
      </div>
    </template>

  </div>
</template>



<script>

import clonedeep from 'lodash.clonedeep';

export default {
  
  name: 'App',

  data (){
    return {
      isActive: false,
      hasMessage: false,
      message: "123",
      newTask: {
        name: "",
        isCompleted: false
      },
      db: {
        tasks: [
          
        ]
      }
    }
  },

  methods: {
    addTask (task) {
      if (!this.newTask.name) return;
      if (this.newTask.name.includes('grocer')) {
        this.hasMessage = true;
        this.message = "this is not a job for Peyton, reserved for Meghan";
        return;
      } else {
        this.message = "";
        this.hasMessage = false
      }
      if (!this.db.tasks.length) {
        task.id = 1;
      } else {
        task.id = this.db.tasks[0].id++
      }
      console.log(this.db.tasks)
      this.db.tasks.push(clonedeep(task));
      this.newTask.name = "";
      this.isActive = false;
      
    }
  }

}
</script>

<style lang="scss">
#app {
  font-family: Avenir, Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  color: #2c3e50;
  margin-top: 100px;
  max-width: 400px;
  margin: 75px auto;

}
#createNewTask {
  background-color: #EDEDED;
    font-size: 22px;
    color: black;
    margin: 1em auto;
    border-radius: 10px;
    height: 75px;
    display: flex;
    align-items: center;
    padding-left: 25px
}
.task {
  background-color: #EDEDED;
    font-size: 18px;
    color: black;
    margin: 1em auto;
    border-radius: 10px;
    height: auto;
    display: flex;
    align-items: center;
    padding: 10px 5px 10px 15px;
}
.button {
  border: none;
  width: 50px;
  height: 50px;
  border-radius: 50px;
  display: flex;
  align-items: center;
  justify-content: center;
}
.bg-orange {
    background-color: #FFB33D;
}
.bg-white {
    background-color: #FFF;
}

#createNewTask {
  display: flex;

}
.column {
  width: 80%;
  &:nth-child(2) {
     width: 20%;
    display: flex;
    justify-content: flex-end;
    padding-right: 10px
  }

}
</style>
