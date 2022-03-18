<template>
  <div>
      <input type="checkbox" v-model="mutableItem.completed"/>
        <span v-if="!editing" :class="{ completed: mutableItem.completed, task: !mutableItem.completed }" >
            {{mutableItem.name}}
            <button  @click="editing = !editing">Rename</button>
            <button @click="removeTask">Remove</button>
        </span>
        <span v-if="editing">
            <input type="text" v-model="newTaskName" @keyup.enter="changeTaskName" />
            <button @click="changeTaskName">Save</button>
            <button @click="removeTask">Remove</button>
        </span>   
  </div>
</template>

<script>
export default {
    props: ['item'],
    data: function(){ // Can't use  the prop item for our v-model so we need to use a new one that is equal to item
        return{
            mutableItem: this.item,
            editing: false,
            newTaskName: ''
        }
    },
    created(){
        this.newTaskName =this.mutableItem.name
    },
    methods: {
        removeTask() {
           this.$emit('remove-task', this.item.id) 
        },
        changeTaskName(){
            this.$emit('change-name', this.mutableItem.id, this.newTaskName)
            this.editing = !this.editing
        }
    }
}
</script>

<style scoped>

button{
    width: 4rem;
    height: 1.7rem;
    font-size: 0.8rem;
    margin: 0.3rem;
    margin-left: 0.8rem;
    border-radius: 1rem;
    border-style: none;
    transition: all .15s ease-in-out;
    
}

button:hover {
    background-color: rgba(0,128,96,1);
    color:beige;
    transform: scale(1.1);
}

input[type=text] {
    width: 15rem;
    height: 1.4rem;
    border-radius: 25px;
    border-style: none;
    margin-left: 0.5rem;
    padding-left: 0.5rem;
}

input[type=checkbox] {
    transform: scale(1.5);

}

.completed {
    text-decoration: line-through;
    color: #008060;
}

.task {
    color: #fff;
    font-weight: 500;
}
</style>