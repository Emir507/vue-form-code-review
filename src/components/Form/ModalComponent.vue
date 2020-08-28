<template>
  <div class="modal__wrap">
    <div class="modal__container">
      <h3>Клиент создан</h3>
      <button @click="close">ОК</button>
    </div>
  </div>
</template>
<script>
import { required, alpha } from 'vuelidate/lib/validators'
export default {
  methods: {
    close() {
      this.$emit('close')
    }
  }
}
</script>
<style lang="scss" scoped>
  .modal__wrap {
    position: fixed;
    z-index: 9998;
    top: 0;
    left: 0;
    width: 100%;
    height: 100%;
    background-color: rgba(0, 0, 0, .5);
    display: flex;
    transition: opacity 0.3s ease;
    justify-content: center;
    align-items: center;
}
  .modal__container {
    width: 300px;
    padding: 20px 30px;
    background-color: #fff;
    border-radius: 3px;
    h3 {
      margin-bottom: 10px;
    }
  }
  button {
    background-color: rgb(147, 245, 147);
    padding: 5px 15px;
    font-family: Avenir, Helvetica, Arial, sans-serif;
    color: #2c3e50;
  }
</style>