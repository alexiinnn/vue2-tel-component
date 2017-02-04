<template>
  <div id="app">
    <tel></tel>
  </div>
</template>
<script>
import Tel from './components/Tel.vue'

export default {
  name: 'app',
  components: {
    Tel
  }
}
</script>

<style scoped>

#app{
  display: flex;
  justify-content: center;
  align-items: center;
}

</style>