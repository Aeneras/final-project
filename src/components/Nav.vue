<template>
  <nav>
    <ul class="ul-nav">
        <li><router-link to="/">Task Manager</router-link></li>
        <li class="log-out-welcome"><p>Welcome, user</p></li>
        <li><button @click="signOut" class="button-log-out">Log out</button></li>
      </ul>
    
  </nav>
</template>

<script setup>
// import PersonalRouter from "./PersonalRouter.vue";
import { useUserStore } from "../stores/user";
import { computed } from "vue";
import { useRouter } from "vue-router";
import { ref } from 'vue';
// import { PersonalRouter } from './PersonalRouter.vue'
//constant to save a variable that will hold the use router method
const route = "/";
const buttonText = "Todo app";
// constant to save a variable that will get the user from store with a computed function imported from vue
// const getUser = computed(() => useUserStore().user);
const getUser = useUserStore().user;
// constant that calls user email from the useUSerStore
const userEmail = getUser.email;
// async function that calls the signOut method from the useUserStore and pushes the user back to the Auth view.
const redirect = useRouter();
const signOut = async () => {
  try{
    // call the user store and send the users info to backend to signOut
    // then redirect user to the homeView
     await useUserStore().signOut();
      // redirects user to the homeView
      redirect.push({ path: "/auth/login" });
  } catch (error) {}
};
</script>

<style>
.navbar-img {
  width: 90px;
}
nav {
  background-color: #EBDEF0;
  border:1px solid;
  box-shadow: 5px 5px 5px black;
  border-radius:50px;
  display: flex;
  width: 100%;
  justify-content: space-around;
  align-items: center;
}
.ul-nav{
  display:flex;
  flex-direction:row;
  justify-content:space-around;
  width:100%;
}
nav ul {
  list-style: none;
  padding-inline-start: 0;
  display: flex;
  flex-direction: column;
  align-items: center;
}
nav ul li{
  text-decoration:none;
  margin:2%
}
.button-log-out{
  background-color:EBF5FB;
  border:1px solid red;
  width:80px;

}
</style>