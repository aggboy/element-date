<template>
  <div class="myel-date-wrapper">
    <div class="el-picker-panel el-date-picker el-popper self-date-cmp"
         x-placement="bottom-start">
      <div class="el-picker-panel__body-wrapper">
        <!---->
        <div class="el-picker-panel__body">
          <!---->
          <div class="el-date-picker__header">
            <button @click.stop="clickPrevYear"
                    type="button"
                    @mouseenter.stop="enterPrevYear"
                    aria-label="前一年"
                    :class="[{'not-allow':notAllowYear},{'el-picker-panel__icon-btn':true},{'el-date-picker__prev-btn':true},{'el-icon-d-arrow-left':true},]"></button>
            <button @click.stop="clickPrevMonth"
                    @mouseenter.stop="enterPrevMonth"
                    type="button"
                    aria-label="上个月"
                    :class="[{'not-allow':notAllowMonth},{'el-picker-panel__icon-btn':true},{'el-date-picker__prev-btn':true},{'el-icon-arrow-left':true}]"></button>
            <span class="el-date-picker__header-label">{{nowYear}} 年</span>
            <span class="el-date-picker__header-label">{{nowMonth}} 月</span>
            <button @click.stop="clickNextYear"
                    aria-label="下一年"
                    class="el-picker-panel__icon-btn el-date-picker__next-btn el-icon-d-arrow-right"></button>
            <button @click.stop="clickNextMonth"
                    type="button"
                    aria-label="下个月"
                    class="el-picker-panel__icon-btn el-date-picker__next-btn el-icon-arrow-right"></button></div>
          <div class="el-picker-panel__content">
            <table cellspacing="0"
                   cellpadding="0"
                   class="el-date-table">
              <tbody>
                <tr class="thead-week-box"
                    :style="{'backgroundColor':bgColor}">
                  <!---->
                  <th class="thead-des">一</th>
                  <th class="thead-des">二</th>
                  <th class="thead-des">三</th>
                  <th class="thead-des">四</th>
                  <th class="thead-des">五</th>
                  <th class="thead-des">六</th>
                  <th class="thead-des">日</th>
                </tr>
                <tr class="el-date-table__row"
                    v-for="(item,index) in monthData"
                    :key="index">

                  <td class="normal"
                      v-for="(jtem,jndex) in item"
                      :key="jndex">
                    <slot :jtem="jtem" />
                  </td>

                </tr>
              </tbody>
            </table>

          </div>
        </div>
      </div>

      <div x-arrow=""
           class="popper__arrow"
           style="left: 35px;"></div>
    </div>
  </div>
</template>

<script>
export default {
  props: {
    listData: {
      type: Array,
      default: function () {
        return []
      }
    },
    needPushEmpty: {
      type: Boolean,
      default: false
    },
    pushEmptyObj: {
      type: Object,
      default: function () {
        return {}
      }
    },
    bgColor: {
      type: String,
      default: '#f7f7f7'
    }
  },
  data () {
    return {
      monthIndex: 0,
      nowYear: new Date().getFullYear(),
      nowMonth: new Date().getMonth() + 1,
      timePickerFlag: false,
      notAllowYear: false,
      notAllowMonth: false,
      monthData: []
    }
  },
  methods: {
    changeDate () {
      this.$emit('changeDate')
    },
    enterPrevYear () {
      let { nowYear, initYear, nowMonth, initMonth } = this
      if (nowYear <= initYear) {
        this.notAllowYear = true
        return false
      }
      if (nowYear + nowMonth <= initYear + initMonth) {
        this.notAllowYear = true
      } else {
        this.notAllowYear = false
      }
    },
    enterPrevMonth () {
      if (this.monthIndex === 0) {
        this.notAllowMonth = true
      } else {
        this.notAllowMonth = false
      }
    },
    clickNextMonth () {
      console.log(this.nowMonth);
      if (this.nowMonth === 12) {
        this.nowMonth = 1
        this.nowYear = this.nowYear + 1
        this.monthIndex = this.monthIndex + 1
        this.changeDate()
      } else {
        this.nowMonth = this.nowMonth + 1
        this.monthIndex = this.monthIndex + 1
        this.changeDate()
      }
    },
    clickPrevMonth () {
      if (this.monthIndex === 0) {
        this.notAllowYear = true
        this.notAllowMonth = true
        return
      }
      if (this.nowMonth === 1) {
        this.nowMonth = 12
        this.nowYear = this.nowYear - 1
        this.monthIndex = this.monthIndex - 1
        this.changeDate()
      } else {
        this.nowMonth = this.nowMonth - 1
        this.monthIndex = this.monthIndex - 1
        this.changeDate()
      }
    },
    clickNextYear () {
      this.monthIndex = this.monthIndex + 12
      this.nowYear = this.nowYear + 1
      this.changeDate()
    },
    clickPrevYear () {
      if (this.nowYear <= this.initYear) {
        this.notAllowYear = true
        return false
      }
      if (this.nowYear + this.nowMonth <= this.initYear + this.initMonth) {
        this.notAllowYear = true
        this.notAllowMonth = true
        return false
      }
      this.monthIndex = this.monthIndex - 12
      this.nowYear = this.nowYear - 1
      this.changeDate()
    },
    // 一维变为二维数组
    chunk (arr, num) {
      const iconsArr = []; // 声明数组
      arr.forEach((item, index) => {
        const page = Math.floor(index / num); // 计算该元素为第几个素组内
        if (!iconsArr[page]) { // 判断是否存在
          iconsArr[page] = [];
        }
        iconsArr[page].push(item);
      });
      return iconsArr;
    },
    // 获取当前月 第一天是 周几
    getMonthFirstDay () {
      //当前月当天日期对象：
      let date = new Date();
      //当前月第一天日期对象：
      date = new Date(date.setDate(1))
      //当前月第一天星期几:
      let weekday = date.getDay();
      return weekday + ''
    },
    // 日期补空
    _pushDay (oneDayObj) {
      let today = this.getMonthFirstDay()
      console.log(today);
      let arr = Object.keys(oneDayObj)
      if (arr.length == 0) {
        return []
      } else {
        let g = { ...oneDayObj }
        let obj = {
          '一': [],
          '二': [g],
          '三': [g, g],
          '四': [g, g, g],
          '五': [g, g, g, g],
          '六': [g, g, g, g, g],
          '日': [g, g, g, g, g, g],
          '1': [],
          '2': [g],
          '3': [g, g],
          '4': [g, g, g],
          '5': [g, g, g, g],
          '6': [g, g, g, g, g],
          '7': [g, g, g, g, g, g],
        }
        return obj[today]
      }


    },
  },
  computed: {
    initYear () {
      return new Date().getFullYear()
    },
    initMonth () {
      return new Date().getMonth() + 1
    },
  },
  watch: {
    listData (newValue) {
      // this.monthData = newValue
      // console.log(newValue);
      let resultArr = newValue
      if (this.needPushEmpty) {
        if (Object.keys(this.pushEmptyObj).length > 0) {
          let pushArr = this._pushDay(this.pushEmptyObj)
          resultArr = [...pushArr, ...resultArr]
        }
      }
      if (resultArr instanceof Array && resultArr.length > 0) {
        if (resultArr[0] instanceof Array) {
          this.monthData = newValue
        } else {
          resultArr = this.chunk(resultArr, 7)
          this.monthData = resultArr
        }

      } else {
        throw '请传入数组'
      }
    }
  },
}
</script>

<style lang="scss" scoped>
.myel-date-wrapper {
  .self-date-cmp {
    position: absolute;
    width: 100%;
    left: 0;
    top: 38px;
    z-index: 3000;
    .el-date-picker__header {
      margin: 12px 60px;
      .not-allow {
        cursor: not-allowed !important;
        &:hover {
          color: #999;
        }
      }
      .el-picker-panel__icon-btn {
        font-size: 14px;
      }
      .el-date-picker__header-label {
        font-size: 18px;
        color: #222;
        font-weight: bold;
      }
    }

    .el-picker-panel__content {
      width: 100% !important;
      margin: 15px 0;
      .thead-week-box {
        background: #f7f7f7;
        .thead-des {
          font-weight: bold;
          font-size: 16px;
          color: #333;
          border-bottom: 0px solid;
        }
      }

      .normal {
      }
    }
  }
}
</style>