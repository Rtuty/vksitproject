<template>
  <div class="home">
    <Encrypter msg="Welcome to Your Vue.js App"/>
    <img alt="Vue logo" width="100" height="100" src="../assets/cyphe.png">
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
