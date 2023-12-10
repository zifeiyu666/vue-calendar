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
    <div class="days"
      @mouseleave="handleMouseLeave">
      <span class="prevMD sel-span"
        v-for='(item, index) in prevMDLen'
        :key='index + "p"'
        @click='handleSelPrevD(monthDays[prevM] - prevLD[thisW] + item)'>{{monthDays[prevM] - prevLD[thisW] + item}}</span>
      <span class="thisMD sel-span"
        :class='{"current": showSelected(index + 1), "range": showRange(index)  }'
        v-for='(item, index) in thisMDLen'
        :key='index + "c"'
        @click='handleSelCurD(index + 1)'
        @mouseenter="handleMouseEnter(index + 1)">{{index + 1}}</span>
      <span class="nextMD sel-span"
        v-for='(item, index) in nextMDLen'
        :key='index + "n"'
        @click='handleSelnextD(index + 1)'>{{index + 1}}</span>
    </div>
    <div class="btns">
      <button @click='clearAllSel'>清除选择</button>
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
    selList: {
      type: Array,
      default() {
        return []
      },
    },
    singleSel: {
      type: Boolean,
      default: true,
    },
    curSel: {
      type: Object,
    },
    selMode: {
      type: String,
      default: 'singleSel',
    },
  },
  created() {
    this.selDayList = this.selList.map(item => {
      item.month = item.month - 1
      return item
    })
    if (this.curSel) {
      this.selDate = new Date(this.curSel.year, this.curSel.month - 1)
      this.selDay = new Date(this.curSel.year, this.curSel.month - 1, this.curSel.day)
    }

    if (this.selMode == 'rangeSel') {
      this.selDay = ''
      this.selDayList = []
    }
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
      rangeDayList: [],
      rangeStartD: '',
      rangeEndD: '',
      mouseEnterActive: false,
    }
  },
  computed: {
    isCurM() {
      return this.selDay ? this.thisM == this.selDay.getMonth() : false
    },
    isCurY() {
      return this.selDay ? this.thisY == this.selDay.getFullYear() : false
    },
    thisY() {
      return this.selDate.getFullYear()
    },
    thisM() {
      return this.selDate.getMonth()
    },
    thisD() {
      if (this.selDay) {
        if (this.selMode == 'mutiSel') {
          this.selDayList.push({ year: this.selDay.getFullYear(), month: this.selDay.getMonth(), day: this.selDay.getDate() })
        }
        if (this.selMode == 'rangeSel') {
          switch (this.selDayList.length) {
            case 0:
              this.mouseEnterActive = true

              this.selDayList.push({ year: this.selDay.getFullYear(), month: this.selDay.getMonth(), day: this.selDay.getDate() })
              break
            case 1:
              this.mouseEnterActive = false

              this.selDayList.push({ year: this.selDay.getFullYear(), month: this.selDay.getMonth(), day: this.selDay.getDate() })
              break
            case 2:
              this.mouseEnterActive = true
              this.selDayList = []
              this.selDayList.push({ year: this.selDay.getFullYear(), month: this.selDay.getMonth(), day: this.selDay.getDate() })
              break
          }
        }
        return this.selDay.getDate()
      } else {
        return ''
      }
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
      //return this.monthDays[this.thisRM]
      // 原： return this.monthDays[this.thisRM]  注：这里有错误，导致每月显示天数会与实际情况对不上。
      //应改为
      return this.monthDays[this.thisRM - 1]
    },
    prevMDLen() {
      return this.prevLD[this.thisW]
    },
    nextMDLen() {
      //return 42 - this.thisMDLen - this.prevMDLen
       if (this.thisMDLen !== undefined && this.prevMDLen !== undefined) {
        return 42 - this.thisMDLen - this.prevMDLen;
      } else {
        return 0; // 或者其他适当的处理
      }
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
    // 单选、多选当前月日期
    handleSelCurD(index) {
      let _that = this
      this.selDay = new Date(this.thisY, this.thisM, index)

      // 多选模式
      if (this.selMode == 'mutiSel') {
        this.selDayList = this.selDayList.filter(item => {
          // debugger
          if (item.year == _that.thisY && item.month == _that.thisM && item.day == _that.thisD) {
            this.selDay = ''
            return false
          }
          return true
        })
      }

      // 范围选择模式
      if (this.selMode == 'rangeSel') {
        console.log(this.rangeStartD)
        console.log(this.rangeEndD)
        switch (this.selDayList.length) {
          case 0:
            this.rangeStartD = this.selDay.getDate() - 1
            break
          case 1:
            this.rangeEndD = this.selDay.getDate() - 1
            break
          case 2:
            this.rangeStartD = ''
            this.rangeEndD = ''
            this.rangeStartD = this.selDay.getDate() - 1
            break
        }
        // this.rangeStartD = index - 1
        this.selDayList = this.selDayList.filter(item => {
          // debugger
          if (item.year == _that.thisY && item.month == _that.thisM && item.day == _that.thisD) {
            this.selDay = ''
            return false
          }
          return true
        })
      }
    },

    // 单选下个月的日期
    handleSelPrevD(index) {
      // this.selDay = new Date(this.thisY, this.prevM, index)
      // this.handlePrevM()
    },

    // 单选上个月的日期
    handleSelnextD(index) {
      // this.selDay = new Date(this.thisY, this.nextM, index)
      // this.handleNextM()
    },

    toggleSelYear() {
      this.showSelYear = !this.showSelYear
    },

    handleSelYear(year) {
      this.selDate = new Date(year, this.thisM)
    },

    // 控制选中状态的展示
    showSelected(index) {
      var sel = false
      // console.log(this.selDayList)
      this.selDayList.forEach(item => {
        if (item.day == index && item.month == this.thisM && item.year == this.thisY) {
          sel = true
        }
      })
      if (this.selMode == 'singleSel') {
        sel = false
      }
      return (this.thisD == index && this.isCurM && this.isCurY) || sel
    },

    clearAllSel() {
      this.selDayList = []
      this.selDay = ''
    },
    // 添加鼠标移入事件
    handleMouseEnter(index) {
      if (this.selDayList.length != 2) {
        this.mouseEnterActive = true
      }
      if (this.mouseEnterActive) {
        this.rangeEndD = index - 1
      }
      // console.log('mouseenter', this.mouseEnterActive, index)
    },
    // 添加鼠标移出事件
    handleMouseLeave(index) {
      if (this.selDayList.length != 2) {
        this.mouseEnterActive = false
      }
      // console.log('mouseenter', this.mouseEnterActive, index)
    },

    showRange(index) {
      // debugger

      let startIndex = Math.min(this.rangeEndD, this.rangeStartD)
      let endIndex = Math.max(this.rangeEndD, this.rangeStartD)
      return (
        startIndex && endIndex && index < endIndex && index > startIndex && (this.mouseEnterActive || this.selDayList.length == 2)
        // (this.selDayList.length == 2 && index >= this.selDayList[0].day && index < this.selDayList[1].day)
      )
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

.range {
  background: #ccc;
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
