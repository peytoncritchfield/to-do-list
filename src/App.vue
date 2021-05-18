<template>
  <div id="app">
    <div id="createNewTask">
      <div class="column">Create New Task</div>
      <div class="column">
        <div class="button bg-orange" @click="isActive = !isActive">+</div>
      </div>
    </div>

    <div v-if="isActive" :key="0">
      <input id="newTaskInput" v-model="newTask.name" />

      <div v-if="hasMessage">{{ message }}</div>

      <button id="submitNewTask" @click="addTask(newTask)">add task</button>
    </div>
    <p>Task</p>

    <template v-for="task in db.tasks">
      <div v-if="!task.isCompleted" class="task" :key="`pending-${task.id}`">
        <div class="column">
          <div class="delete" @click="deleteTask(task)">X</div>
          <input
            v-model="task.name"
            @change="updateTask(task)"
            class="task-input"
          />
        </div>
        <div class="column">
          <div class="button bg-white" @click="updateStatus(task)"></div>
        </div>
      </div>
    </template>

    <p>Completed</p>

    <template v-for="task in db.tasks">
      <div v-if="task.isCompleted" class="task" :key="`completed-${task.id}`">
        <div class="column">
          <div class="delete" @click="deleteTask(task)"> X </div>
          <input
            v-model="task.name"
            @change="updateTask(task)"
            class="task-input"
          />
        </div>
        <div class="column">
          <div class="button bg-orange" @click="updateStatus(task)">
            <img src="./assets/check.png" />
          </div>
        </div>
      </div>
    </template>
  </div>
</template>



<script>
import clonedeep from "lodash.clonedeep";
import axios from "axios";

export default {
  name: "App",

  data() {
    return {
      isActive: false,
      hasMessage: false,
      message: "123",
      newTask: {
        name: "",
        isCompleted: false
      },
      db: {
        tasks: [],
      },
    };
  },
  created() {
    this.loadTasks();
  },

  methods: {
    async loadTasks() {
      let tasks = await axios.get(
        "https://us-central1-to-do-list-peyton.cloudfunctions.net/api/tasks"
      );
      this.db.tasks = tasks.data;
    },
    async addTask(task) {
      if (!this.newTask.name) return;
      console.log(this.db.tasks);
      this.db.tasks.push(clonedeep(task));
      this.isActive = false;
      await axios.post(
        `https://us-central1-to-do-list-peyton.cloudfunctions.net/api/task?id=${task.id}`,
        task
      );
      this.loadTasks();
      this.newTask.name = "";
    },
    async updateStatus(task) {
      task.isCompleted = !task.isCompleted;
      await axios.patch(
        `https://us-central1-to-do-list-peyton.cloudfunctions.net/api/task?id=${task.id}`,
        task
      );
      this.loadTasks();
      console.log(task.isCompleted);
    },
    async updateTask(task) {
      await axios.patch(
        `https://us-central1-to-do-list-peyton.cloudfunctions.net/api/task?id=${task.id}`,
        task
      );
      this.loadTasks();
    },
    async deleteTask(task) {
      await axios.delete(
        `https://us-central1-to-do-list-peyton.cloudfunctions.net/api/task?id=${task.id}`);
      this.loadTasks();
    },
  },
};
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
  background-color: #ededed;
  font-size: 22px;
  color: black;
  margin: 1em auto;
  border-radius: 10px;
  height: 75px;
  display: flex;
  align-items: center;
  padding-left: 25px;
}
.task {
  background-color: #eee;
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
  background-color: mediumaquamarine;
}
.bg-white {
  background-color: #fff;
}

#createNewTask {
  display: flex;
}
.column {
  width: 80%;
  display: flex;
  &:nth-child(2) {
    width: 20%;
    display: flex;
    justify-content: flex-end;
    padding-right: 10px;
  }
}
.task-input {
  border: none;
  outline: none;
  font-size: 18px;
  background-color: unset;
}
.delete {
  color: red;
  display: flex;
  margin-right: 10px;
  align-items: center;
  justify-content: center;
  cursor: pointer;
  border-radius: 50%;
  width: 40px;
  height: 40px;
  
  &:hover {
    background-color: #333;
    color: #fff;
  }
}
</style>
