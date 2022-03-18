<template>
  <div>
      <div class="message">
        {{varMessage}}
      </div>
      <div>
          Tasks done: {{taskCompleted}} of {{items.length}}
      </div>
      <div class="progress-bar">
          <div class="progress-bar-completed" :style="{'width' : barCompleted}"></div>
      </div>
      <button @click="removeCompleted">
          Clear completed
      </button>
  </div>
</template>

<script>
export default {
    props:['items'],
    computed: {
        taskCompleted() { 
            return this.items.filter(item => item.completed).length
        },
        barCompleted() {
            return (this.taskCompleted/this.items.length)*100 + '%'
        },
        varMessage(){
            if(this.taskCompleted > 1 && this.taskCompleted === this.items.length){
                return 'Completed!'
            }
            if(this.taskCompleted === 0 && this.taskCompleted === this.items.length){
                return 'Add some tasks!'
            }
            return ''
        }   
    },
    methods:{
        removeCompleted(){
            this.$emit('remove-completed')
            console.log('Removing')
        }
    }
    
}
</script>

<style scoped>
button {
    height: 2rem;
    width: 8rem;
    font-size: 0.9rem;
    margin: 1rem;
    border-radius: 1rem;
    border-style: none;
    transition: all .15s ease-in-out;
}

button:hover {
    background-color: rgba(0,128,96,1);
    color:beige;
    transform: scale(1.1);
}

.progress-bar {
    position: relative;
    display: block;
    margin-top: 0.5rem;
    margin-left: auto;
    margin-right: auto;
    height: 2rem;
    width: 65vw; 
    max-width: 35rem ;
    border-radius: 1rem;
    border: 1px solid beige;
    background-color: #008080; 
    line-height: 2rem 
}
.progress-bar-completed {
    position: absolute;
    height: 100%;
    border-radius: 1rem;
    background: linear-gradient(90deg, rgba(88,175,137,1) 0%, rgba(84,195,154,1) 27%, rgba(88,255,196,1) 100%);
    transition: 1s;
    text-align: center;
    line-height: 2rem;
}
.message{
    color: beige;
    font-size: 1.5rem;
    margin-bottom: 2rem;
}

</style>