<template>
  <div class="home">
<!--    <img alt="Vue logo" src="../assets/logo.png">-->
    <Encrypter msg="Welcome to Your Vue.js App"/>
  </div>
</template>

<script>
// @ is an alias to /src
import Encrypter from '@/components/Encrypter.vue'

export default {
  name: 'HomeView',
  components: {
    Encrypter
  }
}
</script>
