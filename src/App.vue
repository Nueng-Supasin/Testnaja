<script lang="ts">
  import HelloWorld from './components/HelloWorld.vue'
  import liff from '@line/liff'
  import jwt_decode from 'jwt-decode'

  import { onBeforeMount } from '@vue/runtime-core'
  import { ref } from 'vue'


  export default {
    setup(){
      const userImge = ref<String>('');


      const initLine = () =>{
        liff.init({liffId:'1657884946-MV1Anl8n'},() => {
          if(liff.isLoggedIn()){
            runApp();
          }
          else{
            liff.login();
          }
        },err => console.error(err));
      }

      const runApp = () =>{
        const idToken = liff.getIDToken();
        var decode = jwt_decode(idToken);
        console.log(decode);
        liff.getProfile().then(profile => { 
          console.log(profile)
          userImge.value = profile.pictureUrl;
          
        });
        liff.getProfilePlus();
      }
      const Logout = () =>{
        liff.logout();
        window.location.reload();
      }
      
      onBeforeMount(() => {
        initLine();
      })

      return { initLine, runApp, Logout }
    }
  }
</script>

<template>
  <div>
    <div>
      <img :src="userImge" width="100" height="100">
    </div>
    <a href="https://vitejs.dev" target="_blank">
      <img src="/vite.svg" class="logo" alt="Vite logo" />
    </a>
    <a href="https://vuejs.org/" target="_blank">
      <img src="./assets/vue.svg" class="logo vue" alt="Vue logo" />
    </a>
  </div>
  <HelloWorld msg="Vite + Vue" />
  <button type="button" @click="Logout"> Logout</button>
</template>

<style scoped>
.logo {
  height: 6em;
  padding: 1.5em;
  will-change: filter;
  transition: filter 300ms;
}
.logo:hover {
  filter: drop-shadow(0 0 2em #646cffaa);
}
.logo.vue:hover {
  filter: drop-shadow(0 0 2em #42b883aa);
}
</style>
