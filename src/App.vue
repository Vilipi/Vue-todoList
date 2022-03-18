<template>
  <div id="app">
    <h1 class="title">To-do List</h1>
    <NewItem class="new-item" @new="onNewItem"></NewItem>
    <TodoItem class="todo-item" 
      v-for="item in items" :key="item.id"
      :item="item"
      @remove-task = "onRemoveTask"
      @change-name = "onChangeName">
    </TodoItem>
    <RemainingTasks class="counter"
      :items='items'
      @remove-completed = "onRemoveCompleted">
    </RemainingTasks>
  </div>
</template>

<script>
import TodoItem from './components/TodoItem.vue'
import NewItem from './components/NewItem.vue'
import RemainingTasks from './components/RemainingTasks.vue'

export default {
  name: 'App',
  components: {
    TodoItem,
    NewItem,
    RemainingTasks
  },
  data (){
    return {
      items:[
        // {id: 0, name: 'First task', completed: false},
        // {id: 1, name: 'Second task', completed: true}
      ]
    }
  },
  created: function() {
    let dataDB = JSON.parse(localStorage.getItem('itemsLocal'))
    if(dataDB === null){
      this.items = [];
    }else {
      this.items = dataDB;
    }
  },
  methods: {
    onNewItem(taskname){
      console.log(`Adding ${taskname}`)
      // Assign the last value of the array and if its used we add one more value
      if (taskname === ''){
        //we are not adding any empty task to the array
        return
      }
      let newId = this.items.length === null? this.items.length : Math.max(...this.items.map(item => item.id + 1), 0)
      this.items.push({
        id: newId,
        name: taskname,
        compelted: false
      })
      localStorage.setItem('itemsLocal', JSON.stringify(this.items))
      console.log(newId)
    },
    onRemoveCompleted(){
      this.items = this.items.filter(item => !item.completed)
      localStorage.setItem('itemsLocal', JSON.stringify(this.items))
    },
    onRemoveTask(id){
      this.items = this.items.filter(item => item.id !== id)
      localStorage.setItem('itemsLocal', JSON.stringify(this.items))
    },
    onChangeName(id, newTaskName){
      // First and only element of our array that matched with the provided id will change its name
      this.items.filter(task => task.id === id)[0].name = newTaskName
      localStorage.setItem('itemsLocal', JSON.stringify(this.items)) 
    }
  }
}
</script>

<style>

body{
    background: rgb(0,128,96);
    background: linear-gradient(0deg, rgba(0,128,96,1) 0%, rgba(0,153,115,1) 25%, rgba(0,204,153,1) 100%) no-repeat; 
    height:100%;
    width: 100%;
    margin: 0; 
    background-size:cover;
    min-height: 100vh;
} 

.title {
  margin-bottom: 3rem;
  color:beige;
  font-weight: 400;
  font-size: 2.3rem;
}

#app {
  font-family: Avenir, Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  text-align: center;
  color: #2c3e50;
  margin-top: 12rem;
}

.new-item {
  margin-bottom: 2rem;
}

.todo-item {
  margin-top: 1rem;
}

.counter {
  margin-top: 6rem;
}
</style>
