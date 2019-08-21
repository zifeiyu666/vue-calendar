<template>
  <div class="calendar">
    <div class="weeks">
      <span class="week"
        v-for='(item, index) in weeks'
        :key='index'>{{item}}</span>
    </div>
    <div class="days">
      <span class="prevMD"
        v-for='(item, index) in prevMDLen'
        :key='index'>{{monthDays[prevM] - prevLD[thisW] + item}}</span>
      <span class="thisMD"
        v-for='(item, index) in thisMDLen'
        :key='index'>{{index + 1}}</span>
      <span class="nextMD"
        v-for='(item, index) in nextMDLen'
        :key='index'>{{index + 1}}</span>
    </div>
  </div>
</template>

<script>
export default {
  name: 'calendar',
  props: {
    msg: String,
  },
  data() {
    return {
      weeks: '一二三四五六日'.split(''),
      monthDays: [31, 28, 31, 30, 31, 30, 31, 31, 30, 31, 30, 31],
      prevLD: [6, 0, 1, 2, 3, 4, 5],
      thisY: new Date().getFullYear(),
      thisM: new Date().getMonth(),
      thisD: new Date().getDate(),
    }
  },
  computed: {
    prevM() {
      return this.thisM - 1 < 0 ? 11 : this.thisM - 1
    },
    nextM() {
      return this.thisM + 1 > 11 ? 0 : this.thisM + 1
    },
    thisW() {
      return new Date(this.thisY, this.thisM, 1).getDay()
    },
    thisRM() {
      return this.thisM + 1
    },
    thisMDLen() {
      return this.monthDays[this.thisRM]
    },
    prevMDLen() {
      return this.prevLD[this.thisW]
    },
    nextMDLen() {
      return 42 - this.thisMDLen - this.prevMDLen
    },
  },
}
</script>

<style scoped lang='scss'>
.calendar {
  display: inline-block;
  box-shadow: 1px 1px 6px #eee;
}
.weeks,
.days {
  width: 280px;
  display: flex;
  flex-wrap: wrap;
  span {
    flex: 0 0 40px;
    height: 40px;
    line-height: 40px;
  }
  .prevMD,
  .nextMD {
    background: #eee;
  }
}
</style>
