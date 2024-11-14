<!-- option api -->

<script>
export default {
  data(){
     return {
      name: 'john doe',
      status: 'pending',
      tasks: ['task1','task2','task3','task4'],
      link: "https://vuejs.org/guide/quick-start.html"
     };
  },
  methods:
  {
    changeStatus(){
      if(this.status === 'active'){
        this.status = 'pending';
      }else if(this.status ==='pending'){
        this.status = 'inactive';
      }else{this.status = 'active';

      }
    },
  },
};

</script>



<template>
   <h1>{{ name }}</h1>
   <p v-if="status === 'active'">user is active</p>
   <p v-else-if="status === 'pending'">user is pending</p>
   <p v-else>user is inactive</p>

   <h3>Tasks:</h3>
    <ul>
      <li v-for="task in tasks" :key="task">{{ task }}</li>
    </ul>
    <!-- <a v-bind:href="link">Vue.js Documentation</a> -->
    <a :href="link">Vue.js Documentation</a>
    <br/>
    <!-- <button v-on:click="changeStatus">change status</button> -->
     <!-- shorter way to do event -->
    <button @click="changeStatus">change status</button>


</template>










<!-- compositional api -->

<script>
import { onMounted, ref } from 'vue';

export default{
  setup() {
    const name = ref(' john doe');
    const status = ref('active');
    const tasks = ref([ 'task1','task2','task3']);
    const newTask = ref('')

    const changeStatus = () =>{
      if(status.value === 'active'){
        status.value = 'pending';
      }else if(status.value ==='pending'){
        status.value = 'inactive';
      }else{status.value = 'active';

      }
    }
    const addTask = () =>{

      if(newTask.value.trim() !== ''){
        tasks.value.push(newTask.value);
        newTask.value = '';
      }
    };
    const deleteTask = (index) =>{
      tasks.value.splice(index,1);

    };
// called when component is mounted
    onMounted(async () => {
      try{
        const response = await fetch('https://jsonplaceholder.typicode.com/todos');
        const data = await response.json();
        tasks.value = data.map((task) => task.title);
      } catch (error) {
        console.log("error fetching data");
      }
    })

    return {
      name,
      status,
      tasks,
      changeStatus,
      newTask,
      addTask,
      deleteTask
    };
  },
};
</script>



<template>
    <h1>{{ name }}</h1>
   <p v-if="status === 'active'">user is active</p>
   <p v-else-if="status === 'pending'">user is pending</p>
   <p v-else>user is inactive</p>
   
  <form @submit.prevent="addTask">
    <label for="newTask">Add task</label>
    <input type="text" id="newTask" v-model="newTask" name="newTask"/>
    <button type="submit">Add task</button>
  </form>

   <h3>Tasks:</h3>
    <ul>
      <li v-for="(task, index) in tasks" :key="task">
       <span>
        {{ task }}
       </span>
       <button @click="deleteTask(index)">x</button>
      </li>
    </ul>
    <br/>
    <!-- <button v-on:click="changeStatus">change status</button> -->
     <!-- shorter way to do event -->
    <button @click="changeStatus">change status</button>
</template>


