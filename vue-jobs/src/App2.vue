<script setup>
import {onMounted, ref} from 'vue';
const name = ref('Arnold Sal');
const status = ref('active');
const tasks = ref(['Task One', 'Task Two', 'Tast Three']);
const newTask=ref('');

const toggleStatus=()=>{
  if (status.value == 'active'){
    status.value = 'pending';
  }else if(status.value ==='pending'){
    status.value='inactive';
  }else{
    status.value = 'active';
  }
};
const addTask =() =>{
if(newTask.value.trim() !== '') {
  tasks.value.push(newTask.value);
  newTask.value = '';
}
};
const deleteTask = (index) =>{
  tasks.value.splice(index,1 ); 
};

onMounted(async() => {
  try{
    const response = await fetch('https://jsonplaceholder.typicode.com/todos');
    const data = await response.json();
    tasks.value = data.map((task) => task.title) 
  } catch (error){
    console.log('Error fetching logs');
  }
});

</script>

<template>
<h1>{{ name }}</h1><br>
<p v-if="status === 'active'">User is active</p>
<p v-else-if="status === 'pending'">User is pending</p>
<p v-else>User is inactive</p>
<br>
<form @submit.prevent="addTask">
  <label for="newTask">Add Task</label>
  <input type="text" id="newTask" name="newTask" v-model="newTask" />
  <button type="submit">Submit</button>
</form>
<h3>Tasks:</h3><br> 
<ul>
  <li v-for="(task, index) in tasks":key="task">
    <span>
        {{ task }}
    </span>
  <button @click="deleteTask(index)">x</button>
  </li>
</ul>
<br />
<button @click="toggleStatus">Change Status</button>

</template>

