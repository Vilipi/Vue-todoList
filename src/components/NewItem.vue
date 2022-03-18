<template>
 <div>
  <input class="input"
    type="text" required
    placeholder="Introduce a new task" 
    v-model="taskName"
    @keyup.enter="onNewItem"
    />
  <button class="input-btn" @click="onNewItem">+</button>
  <transition v-if="warningMessage" name="fade" mode="out-in">
        <div class="warning">
            <span>Task field should not be empty</span>
        </div>
    </transition> 
 </div>  
</template>

<script>
export default {
    data() {
        return {
            // New input value will be saved here
            taskName: '',
            warningMessage: false
        }
    },
    methods: {
        onNewItem() {
            if(this.taskName === ''){
                this.warningMessage = true;
                setTimeout(() => {
                this.warningMessage = false
                }, 4000)
                return
            }
            this.$emit('new', this.taskName )
            this.taskName = '' //Cleaning input bar when is sent to father component   
        }
    }
}
</script>

<style>
.input {
    width: 17rem;
    height: 3.5vh;
    border-radius: 20px;
    border-style: none;
    margin-right: 0.4rem;
    padding-left: 0.8rem;
}
.input-btn{
    width: 2rem;
    height: 2rem;
    border-radius: 100px;
    border-style: none;
    background-color:#fff;
    transition: all .2s ease-in-out;
}
.input-btn:hover{
    background-color: rgba(0,128,96,1);
    color:beige;
    transform: scale(1.1);
}
.warning{
    margin-top: 1rem;
    font-size: 1.5rem;
}
.warning span{
    transition: all 0.2s;
    padding: 0 1rem;
    border-radius: 0.5rem;
    background-color:#CD5C5C;
    color:#fff; 
}

.fade-enter-active,
.fade-leave-active {
  transition: opacity 1s ease;
}

.fade-enter,
.fade-leave-to {
  opacity: 0;
}
</style>