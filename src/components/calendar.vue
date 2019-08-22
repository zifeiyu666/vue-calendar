<template>

  <div class="calendar"
    :class='theme'>
    <div class="tools">
      <span class="leftArr"
        @click='handlePrevM()'>&lt;</span>
      <div class="center"
        @click='toggleSelYear'>
        <span class="year">{{thisY}}</span>
        <span>-</span>
        <span class="month">{{thisRM}}</span>
      </div>
      <span class="rightArr"
        @click='handleNextM()'>&gt;</span>
      <div class="selyear-wrap"
        v-show='showSelYear'>
        <span class='sel-span'
          v-for='(item, index) in 25'
          :key='index + "y"'
          @click='handleSelYear(thisY - 12 + index)'
          :class='{current: thisY == thisY - 12 + index}'>{{thisY - 12 + index}}</span>
      </div>
    </div>
    <div class="weeks">
      <span class="week"
        v-for='(item, index) in weeks'
        :key='index'>{{item}}</span>
    </div>
    <div class="days">
      <span class="prevMD sel-span"
        v-for='(item, index) in prevMDLen'
        :key='index + "p"'
        @click='handleSelPrevD(monthDays[prevM] - prevLD[thisW] + item)'>{{monthDays[prevM] - prevLD[thisW] + item}}</span>
      <span class="thisMD sel-span"
        :class='{"current": showSelected(index + 1)  }'
        v-for='(item, index) in thisMDLen'
        :key='index + "c"'
        @click='handleSelCurD(index + 1)'>{{index + 1}}</span>
      <span class="nextMD sel-span"
        v-for='(item, index) in nextMDLen'
        :key='index + "n"'
        @click='handleSelnextD(index + 1)'>{{index + 1}}</span>
    </div>
  </div>
</template>

<script>
export default {
  name: 'calendar',
  props: {
    msg: String,
    theme: {
      type: String,
      default: 'red',
    },
  },
  data() {
    return {
      weeks: '一二三四五六日'.split(''),
      monthDays: [31, 28, 31, 30, 31, 30, 31, 31, 30, 31, 30, 31],
      prevLD: [6, 0, 1, 2, 3, 4, 5],
      selDate: new Date(),
      selDay: new Date(),
      selDayList: [],
      showSelYear: false,
    }
  },
  computed: {
    isCurM() {
      return this.thisM == this.selDay.getMonth()
    },
    isCurY() {
      return this.thisY == this.selDay.getFullYear()
    },
    thisY() {
      return this.selDate.getFullYear()
    },
    thisM() {
      return this.selDate.getMonth()
    },
    thisD() {
      this.selDayList.push({ year: this.selDay.getFullYear(), month: this.selDay.getMonth(), day: this.selDay.getDate() })
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
    // 下一个月
    handleNextM() {
      // debugger
      var curM = this.thisM + 1 > 11 ? 0 : this.thisM + 1
      this.selDate = new Date(this.thisY, curM)
    },
    // 上一个月
    handlePrevM() {
      var curM = this.thisM - 1 < 0 ? 11 : this.thisM - 1
      this.selDate = new Date(this.thisY, curM)
    },
    // 单选当前月日期
    handleSelCurD(index) {
      this.selDay = new Date(this.thisY, this.thisM, index)
    },

    // 单选下个月的日期
    handleSelPrevD(index) {
      this.selDay = new Date(this.thisY, this.prevM, index)
      this.handlePrevM()
    },

    // 单选上个月的日期
    handleSelnextD(index) {
      this.selDay = new Date(this.thisY, this.nextM, index)
      this.handleNextM()
    },

    toggleSelYear() {
      this.showSelYear = !this.showSelYear
    },

    handleSelYear(year) {
      this.selDate = new Date(year, this.thisM)
    },

    showSelected(index) {
      return this.thisD == index && this.isCurM && this.isCurY
    },
  },
}
</script>

<style scoped lang='scss'>
@import '@/assets/style/index.scss';
.calendar {
  display: inline-block;
  box-shadow: 1px 1px 6px #eee;
}
.weeks {
  border-bottom: 1px solid #eee;
}
.current {
  color: #fff;
}
.tools,
.weeks,
.days {
  width: 280px;
  display: flex;
  flex-wrap: wrap;
  span {
    flex: 0 0 38px;
    height: 38px;
    line-height: 38px;
    margin: 1px;
    cursor: pointer;
  }
  .prevMD,
  .nextMD {
    background: #eee;
    color: #aaa;
  }
}
.tools {
  position: relative;
  justify-content: space-around;
  span {
    cursor: pointer;
  }
  .selyear-wrap {
    display: flex;
    flex-wrap: wrap;
    position: absolute;
    width: 100%;
    top: 40px;
    background: #fff;
    height: 280px;
    span {
      flex: 1 0 19%;
    }
  }
}

// theme
.red {
  .sel-span.current {
    background: $r-bg;
    // outline: 2px solid $r-bg;
    // border-radius: 50%;
  }
  .sel-span:hover {
    outline: 2px solid $r-bg;
  }
}
</style>
