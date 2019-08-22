<template>
  <div id="app">
    <calendar :selList="list"
      :singleSel='true' />
  </div>
</template>

<script>
import Calendar from './components/calendar.vue'

export default {
  name: 'app',
  components: {
    Calendar,
  },
  data() {
    return {
      list: [
        {
          year: '2019',
          month: '8',
          day: '21',
        },
        {
          year: '2019',
          month: '8',
          day: '20',
        },
        {
          year: '2019',
          month: '7',
          day: '10',
        },
      ],
    }
  },
}
</script>

<style>
#app {
  font-family: 'Avenir', Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  text-align: center;
  color: #2c3e50;
  margin-top: 60px;
}
</style>
