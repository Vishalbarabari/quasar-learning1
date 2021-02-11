<template>
  <q-page class="bg-grey-3 column">
   <div class ="div.row.q-pa-sm bg-primary">
     <q-input class="col" 
     square @keyup.enter="addtask"
     filled  v-model="newtask" placeholder="Add task"
       bg-color="white" dense>
     

        <template v-slot:append>
          <q-btn @click="addtask" round dense flat icon="add" />
        </template>
      </q-input>
     </div> 

    <q-list class="bg-white" separator bordered>
      <q-item v-for="(task,index) in tasks" 
        :key="task.title" 
        @click="task.done = !task.done" 
        :class="{'done bg-red-1':task.done}"
        clickable v-ripple>
        <q-item-section avatar>
          <q-checkbox v-model="task.done" color="primary"  class="no-pointer-events"/>
        </q-item-section>
        <q-item-section>
          <q-item-label>{{task.title}}</q-item-label>
        </q-item-section> 
        <q-item-section
        v-if="task.done" side>
           <q-btn @click.stop="deletetask(index)" flat round  dense color="primary" icon="delete" />
        </q-item-section> 
      </q-item>

      <div v-if="!tasks.length" class="no-tasks absolute-center">
        <div class="text-h5 text-primary text-center">
          No tasks
        </div>
        </div> 

      
    </q-list>
  </q-page>
</template>

<script>
export default {
  data(){
    return{
      newtask:"",
      tasks:[
        
        

      ]
    }
  },
  methods:{
    deletetask(index){
      this.$q.dialog({
        title: 'Confirm',
        message: 'do you really want to delete?',
        cancel: true,
        persistent: true
      }).onOk(() => {
        this.tasks.splice(index, 1);
        this.$q.notify('Task deleted')
      })
      
    },
    addtask(){
      this.tasks.push({
        title:this.newtask,
        done:false
      })
      this.newtask=""
    }
  }
}
</script>

<style lang="scss">
  .done{
    .q-item__label{
      text-decoration: line-through;
      color: darkgray;
    }
  }

</style>