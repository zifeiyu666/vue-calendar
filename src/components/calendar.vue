<template>

  <div class="calendar">
    <div class="tools">
      <span class="leftArr"
        @click='handlePrevM()'>&lt;</span>
      <div class="center">
        <span class="year">{{thisY}}</span>
        <span>-</span>
        <span class="month">{{thisRM}}</span>
      </div>
      <span class="rightArr"
        @click='handleNextM()'>&gt;</span>
    </div>
    <div class="weeks">
      <span class="week"
        v-for='(item, index) in weeks'
        :key='index'>{{item}}</span>
    </div>
    <div class="days">
      <span class="prevMD"
        v-for='(item, index) in prevMDLen'
        :key='index + "p"'>{{monthDays[prevM] - prevLD[thisW] + item}}</span>
      <span class="thisMD"
        :class='{"current": (thisD == index + 1) && isCurM }'
        v-for='(item, index) in thisMDLen'
        :key='index + "c"'
        @click='handleSelCurD(index)'>{{index + 1}}</span>
      <span class="nextMD"
        v-for='(item, index) in nextMDLen'
        :key='index + "n"'>{{index + 1}}</span>
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
      selDate: new Date(),
      selDay: new Date(),
    }
  },
  computed: {
    isCurM() {
      return this.thisM == this.selDay.getMonth()
    },
    thisY() {
      return this.selDate.getFullYear()
    },
    thisM() {
      return this.selDate.getMonth()
    },
    thisD() {
      return this.selDay.getDate()
    },
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
  methods: {
    handleNextM() {
      // debugger
      var curM = this.thisM + 1 > 11 ? 0 : this.thisM + 1
      this.selDate = new Date(this.thisY, curM)
    },
    handlePrevM() {
      var curM = this.thisM - 1 < 0 ? 11 : this.thisM - 1
      this.selDate = new Date(this.thisY, curM)
    },
    handleSelCurD(index) {
      this.selDay = new Date(this.thisY, this.thisM, index + 1)
    },
  },
}
</script>

<style scoped lang='scss'>
.calendar {
  display: inline-block;
  box-shadow: 1px 1px 6px #eee;
}
.weeks {
  border-bottom: 1px solid #eee;
}
.current {
  background: red;
  color: #fff;
}
.tools,
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
    color: #aaa;
  }
}
.tools {
  justify-content: space-around;
  // .year {
  //   font-size: 1em;
  // }
  // .month {
  //   font-size: 0.8em;
  //   color: #999;
  // }
}
</style>
