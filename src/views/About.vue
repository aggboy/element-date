<template>
  <div class="open-time">
    <!-- 
      needPushEmpty：是否需要每个月第一天前面补空对象 如7月1号是星期三 则 星期一和星期二 补空
      pushEmptyObj：需要补空对象 的 {} 参数
      bgColor: 头部背景颜色
     -->
    <myDate :listData="monthData"
            v-slot:default="{jtem}"
            :bgColor="'green'"
            needPushEmpty
            :pushEmptyObj='pushEmptyObj'
            @changeDate='getMonthData'>
      <!-- 插槽内容  自定义显示-->
      <div class="font-box"
           @click.stop="selectExceptTime(jtem)">
        <b v-if='jtem.usableOrNot'>
          <span :class="[{'day':true},{'not-allow':jtem.full}]">{{jtem.date | filterDateToDay(jtem.date)}}</span>
          <i class="des full"
             v-if='jtem.full'>
            <svg-icon iconClass="man-font-red"></svg-icon>
          </i>
          <i class="des"
             v-if='!jtem.full'>余{{jtem.remainNum}}</i>
        </b>
        <b v-if='!jtem.usableOrNot'>
          <span class="day not">{{jtem.date | filterDateToDay(jtem.date)}}</span>
        </b>
      </div>
    </myDate>
  </div>
</template>
<script>
import myDate from '../components/dateCmp'
export default {
  data () {
    return {
      monthData: [{ "date": "2020-07-01", "full": false, "remainNum": 5, "totalNum": 5, "usableOrNot": false, "usedNum": 0, "week": "三" }, { "date": "2020-07-02", "full": false, "remainNum": 13, "totalNum": 13, "usableOrNot": false, "usedNum": 0, "week": "四" }, { "date": "2020-07-03", "full": true, "remainNum": 0, "totalNum": 0, "usableOrNot": false, "usedNum": 0, "week": "五" }, { "date": "2020-07-04", "full": true, "remainNum": 0, "totalNum": 0, "usableOrNot": false, "usedNum": 0, "week": "六" }, { "date": "2020-07-05", "full": true, "remainNum": 0, "totalNum": 0, "usableOrNot": false, "usedNum": 0, "week": "日" }, { "date": "2020-07-06", "full": true, "remainNum": 0, "totalNum": 0, "usableOrNot": false, "usedNum": 0, "week": "一" }, { "date": "2020-07-07", "full": true, "remainNum": 0, "totalNum": 0, "usableOrNot": false, "usedNum": 0, "week": "二" }, { "date": "2020-07-08", "full": false, "remainNum": 5, "totalNum": 5, "usableOrNot": false, "usedNum": 0, "week": "三" }, { "date": "2020-07-09", "full": false, "remainNum": 13, "totalNum": 13, "usableOrNot": true, "usedNum": 0, "week": "四" }, { "date": "2020-07-10", "full": true, "remainNum": 0, "totalNum": 0, "usableOrNot": false, "usedNum": 0, "week": "五" }, { "date": "2020-07-11", "full": true, "remainNum": 0, "totalNum": 0, "usableOrNot": false, "usedNum": 0, "week": "六" }, { "date": "2020-07-12", "full": true, "remainNum": 0, "totalNum": 0, "usableOrNot": false, "usedNum": 0, "week": "日" }, { "date": "2020-07-13", "full": true, "remainNum": 0, "totalNum": 0, "usableOrNot": false, "usedNum": 0, "week": "一" }, { "date": "2020-07-14", "full": true, "remainNum": 0, "totalNum": 0, "usableOrNot": false, "usedNum": 0, "week": "二" }, { "date": "2020-07-15", "full": false, "remainNum": 5, "totalNum": 5, "usableOrNot": true, "usedNum": 0, "week": "三" }, { "date": "2020-07-16", "full": false, "remainNum": 12, "totalNum": 13, "usableOrNot": true, "usedNum": 1, "week": "四" }, { "date": "2020-07-17", "full": true, "remainNum": 0, "totalNum": 0, "usableOrNot": false, "usedNum": 0, "week": "五" }, { "date": "2020-07-18", "full": true, "remainNum": 0, "totalNum": 0, "usableOrNot": false, "usedNum": 0, "week": "六" }, { "date": "2020-07-19", "full": true, "remainNum": 0, "totalNum": 0, "usableOrNot": false, "usedNum": 0, "week": "日" }, { "date": "2020-07-20", "full": true, "remainNum": 0, "totalNum": 0, "usableOrNot": false, "usedNum": 0, "week": "一" }, { "date": "2020-07-21", "full": true, "remainNum": 0, "totalNum": 0, "usableOrNot": false, "usedNum": 0, "week": "二" }, { "date": "2020-07-22", "full": false, "remainNum": 5, "totalNum": 5, "usableOrNot": true, "usedNum": 0, "week": "三" }, { "date": "2020-07-23", "full": false, "remainNum": 13, "totalNum": 13, "usableOrNot": true, "usedNum": 0, "week": "四" }, { "date": "2020-07-24", "full": true, "remainNum": 0, "totalNum": 0, "usableOrNot": false, "usedNum": 0, "week": "五" }, { "date": "2020-07-25", "full": true, "remainNum": 0, "totalNum": 0, "usableOrNot": false, "usedNum": 0, "week": "六" }, { "date": "2020-07-26", "full": true, "remainNum": 0, "totalNum": 0, "usableOrNot": false, "usedNum": 0, "week": "日" }, { "date": "2020-07-27", "full": true, "remainNum": 0, "totalNum": 0, "usableOrNot": false, "usedNum": 0, "week": "一" }, { "date": "2020-07-28", "full": true, "remainNum": 0, "totalNum": 0, "usableOrNot": false, "usedNum": 0, "week": "二" }, { "date": "2020-07-29", "full": false, "remainNum": 5, "totalNum": 5, "usableOrNot": true, "usedNum": 0, "week": "三" }, { "date": "2020-07-30", "full": false, "remainNum": 13, "totalNum": 13, "usableOrNot": true, "usedNum": 0, "week": "四" }, { "date": "2020-07-31", "full": true, "remainNum": 0, "totalNum": 0, "usableOrNot": false, "usedNum": 0, "week": "五" }],
      pushEmptyObj: {
        date: "",
        full: false,
        remainNum: 0,
        totalNum: 0,
        usableOrNot: false,
        usedNum: 0,
        week: "二",
      }

    }
  },
  mounted () {
    setTimeout(() => {
      this.initDate()
    }, 1000);
  },
  methods: {
    getMonthData () {
      console.log('可以获取新数据了');
    },
    initDate () {
      setTimeout(() => {
        this.monthData = [...this.monthData]
        // this.chunk(this.monthData, 7)
      }, 1000);
    },
    selectExceptTime (value) {
      console.log(value, '获取到某一提案的数据了');
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
  },
  filters: {
    filterDateToDay (day) {
      let str = ''
      if (day) {
        str = Number(day.substring(day.lastIndexOf('-') + 1))
      }
      return str
    }
  },
  components: {
    myDate,
  },
}
</script>

<style lang='scss' scoped>
.open-time {
  position: relative;
  width: 400px;
  margin-top: 40px;
  .font-box {
    b {
      font-weight: normal;
    }
    .day {
      font-weight: bold;
      color: #333333;
      font-size: 14px;
      &:hover {
        color: #0079fd;
      }
    }
    .des {
      font-style: normal;
      display: inline-block;
      margin-top: 12px;
      color: #007aff;
      font-size: 10px;
    }
    .not {
      color: #9f9f9f;
      font-weight: bold;
      font-size: 14px;
      cursor: not-allowed;
      &:hover {
        color: #9f9f9f;
      }
    }
    .not-allow {
      cursor: not-allowed !important;
    }

    .full {
      cursor: not-allowed;
      font-size: 18px;
    }
  }
}
</style>
