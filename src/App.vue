<template>
  <div id="app">
    <div class="main-container">
      <ProdSignupForm />
    </div>
    
  </div>
</template>

<script>
import ProdSignupForm from './components/ProdSignupForm.vue'

export default {
  name: 'App',
  components: {
    ProdSignupForm
  }
}
</script>

<style>
#app {
  font-family: Avenir, Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  color: #2c3e50;
  margin-top: 60px;
}
.main-container{
  width: 420px;
  margin: 0 auto;
}
</style>
