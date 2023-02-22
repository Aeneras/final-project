<template>
  <div class="father">
    <div class="container">
      
      <h3 :class="props.task.is_complete ? 'case1': 'case2'">{{task.title}}</h3>
    <p :class="props.task.is_complete ? 'case1': 'case2'">{{ task.description }}</p>
      <!-- <p>{{ task.is_complete }}</p> -->
      <button @click="showModal = true">Delete </button>
      <!-- Modal -->
      <div class="modal" v-if="showModal">
        <h2>Are you sure?</h2>
        <p>But really really sure?</p>
        <button @click="deleteTask">Yes!Burn it all!</button>
        <button @click="showModal = false">No,plis! I am scared!</button>
        
      </div>

      <button @click="completedTask">Completed </button>
      <!-- <h3 :class="props.task.is_complete ? 'case1': 'case2'">{{task.title}}</h3>
    <p :class="props.task.is_complete ? 'case1': 'case2'">{{ task.description }}</p> -->
    
    <button @click="showInput">Edit</button>
</div>
<div class="edit">
    <div v-if="inputContainer">
      <input type="text" v-model="currentTaskTitle" />
      <input type="text" v-model="currentTaskDescription" />
      <button @click="editTask">Edit Task</button>
      </div>
    </div>
  </div>
  </template>
  
  <script setup>
  import { ref } from "vue";
  import { useTaskStore } from "../stores/task";
  import { supabase } from "../supabase";

  // definir emits para pasar lógica y eventos hacia componentes padres
  const emit = defineEmits(["taskComplete", "editChild"]);
  

  const showModal=false;
  // variable para usar tienda de tarea facil
  const taskStore = useTaskStore();
  // variable para recibir informacion de la tarea mediante prop como .Objeto
  const props = defineProps({
    task: Object,
  });
  // funcion para mostrar y ocultar inputs
  const inputContainer = ref(false);
  const currentTaskTitle = ref("");
  const currentTaskDescription = ref("");
  const showInput = () => {
    console.log("click");
    inputContainer.value = !inputContainer.value;
    currentTaskTitle.value = props.task.title;
    currentTaskDescription.value = props.task.description;
  };
  // funcion con validacion + envio de datos y eventos mediane emit
  const editTask = () => {
    if (currentTaskTitle.value.length === 0 || currentTaskDescription.value.length === 0) {
      alert("Title or Description can not be empty");
    } else {
      taskStore.editTaskSupabase(currentTaskTitle.value, props.task.id, currentTaskDescription.value);
      emit("editChild");
    }
  };



  // Función para borrar la tarea a través de la store. El problema que tendremos aquí (y en NewTask.vue) es que cuando modifiquemos la base de datos los cambios no se verán reflejados en el v-for de Home.vue porque no estamos modificando la variable tasks guardada en Home. Usad el emit para cambiar esto y evitar ningún page refresh.
  const deleteTask = async () => {
    await taskStore.deleteTask(props.task.id);
  };
  // FUNCION TO COMPLETE TASK Y ENVIAR INFO DE TAREA A HOME
  const completedTask = () =>{
    // chequear que este conectada
    console.log("Completada!")
    console.log(props.task)
    // pasar info de tarea con emit 
    emit("taskComplete", props.task);
    
  };

  
  </script>
  
  <style scoped>
  .modal{
    display:flex;
    flex-direction:column;
    align-items:center;
    justify-content:center;
    width:20px;
    height:200px;
    background: #CFD8DC ;
    border:1px solid black;
    border-radius:10px;
    margin-top:10px;
   

  }
  .modal h2{
    margin-bottom:3px;
    color:#5DADE2;
  }
  .modal p{
    color:red;
  }
  .completedTask{
    background:#D1C4E9
  }
</style>
  
  <!--
  **Hints**
  1. ref() or reactive() can be used here to store the following, think if you want to store them either individually or
  like an object, up to you.
  2. Data properties need here are the following: a boolean to store a false**Important variable, a string to store an error,
  a string to store the value of the task that the user can edit, an initial false boolean to hide the inputFIeld used to edit
  the new task detail or details[this is in reference of the task title and the task description].
  3. Store the custom emit events that will be used to call the functions of the homeView for editing, deleting and toggling the
  status[completed, not complted] of the taskItem.
  4. Function to handle the error with the data properties used to control the error and the the boolean controlling the boolean
  empty variable.
  5. Function to handle the edit dialogue where the inputField is displayed and the string used to store the value of the
  inputField will be used here to save the value as a prop on this function.
  6. Function to emmit a custom event emit() that takes 2 parameters a name for the custom event and the value that will be
  send via the prop to the parent component. This function can control the toggle completion of the task on the homeview.
  7. Function to edit the task information that you decided that the user can edit. This function's body takes in a conditional
  that first checks if the value of the task [either title and description or just title] is empty which if true it runs the
  function used to handle the error on hint4. Else, the conditional sets the first mentioned boolean data property on hint2
  back to its inital boolean value to hide the error message and repeat the same for the data property mentioned 4th on hint2;
  a constant that stores an object that holds the oldValue from the prop item and the value of the task coming from the data
  property mentioned 3rd on hint2; a custom event emit() that takes 2 parameters a name for the custom event and the value
  from the object used on this part of the conditional and lastly this part of the conditional sets the value of input field
  to an empty string to clear it from the ui.
  8. Function to emmit a custom event emit() that takes 2 parameters a name for the custom event and the value that will be
  send via the prop to the parent component. This function can control the removal of  the task on the homeview.
  -->