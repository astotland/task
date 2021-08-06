<template>
  <div id="app">
      <Calendar 
       date="2022-01-15"
      ></Calendar>
  </div>
</template>

<script>
import Calendar from './components/Calendar.vue'


export default {
  name: 'App',
  components: {
    Calendar  
  },
  data: () => ({
    test: 'name'
  }),
}
</script>

<style>
/* #app {

} */
</style>
