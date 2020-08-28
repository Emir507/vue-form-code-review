<template>
  <div id="app">
    <FormComponent />
  </div>
</template>

<script>
import FormComponent from './views/FormComponent.vue'

export default {
  name: 'App',
  components: {
    FormComponent
  },
  data() {
    return {
      show: false
    }
  },
  methods: {
    change() {
      this.show = !this.show
    }
  }
}
</script>

<style lang="scss">
body {
  margin: 0;
}
h1, h2, h3, h4, h5, h6 {
  margin: 0;
}
button {
  background-color: transparent;
  outline: none;
  border: none;
}
input {
  outline: none;
  border: 1px solid #000000;
  border-radius: 5px;
}
#app {
  background-color: rgb(231, 216, 195);
  position: relative;
  font-family: Avenir, Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  text-align: center;
  color: #2c3e50;
}

</style>
