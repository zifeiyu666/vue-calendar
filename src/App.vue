<template>
  <div id="app">
    <calendar :selList="list"
      :curSel='curSel'
      :selMode='selMode' />
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
      selMode: 'rangeSel', // singleSel, mutiSel, rangeSel
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
      curSel: {
        year: '2019',
        month: '8',
        day: '23',
      },
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
