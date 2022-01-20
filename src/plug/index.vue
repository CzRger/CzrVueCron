<template>
  <div class="cron">
    <div class="button" @click="initVal">反解析Cron表达式</div>
    <div class="tabs">
      <div class="tab" :class="{ active: mode === 1 }" @click="mode = 1">秒</div>
      <div class="tab" :class="{ active: mode === 2 }" @click="mode = 2">分钟</div>
      <div class="tab" :class="{ active: mode === 3 }" @click="mode = 3">小时</div>
      <div class="tab" :class="{ active: mode === 4 }" @click="mode = 4">日</div>
      <div class="tab" :class="{ active: mode === 5 }" @click="mode = 5">月</div>
      <div class="tab" :class="{ active: mode === 6 }" @click="mode = 6">周</div>
      <div class="tab" :class="{ active: mode === 7 }" @click="mode = 7">年</div>
    </div>
    <div class="content">
      <secondsCom :val.sync="secondsVal" v-show="mode === 1"/>
      <minuteCom :val.sync="minuteVal" v-show="mode === 2"/>
      <hourCom :val.sync="hourVal" v-show="mode === 3"/>
      <dayCom :val.sync="dayVal" v-show="mode === 4" @reset="v => weekVal = v"/>
      <monthCom :val.sync="monthVal" v-show="mode === 5"/>
      <weekCom :val.sync="weekVal" v-show="mode === 6" @reset="v => dayVal = v"/>
      <yearCom :val.sync="yearVal" v-show="mode === 7"/>
    </div>
  </div>
</template>

<script>
  import secondsCom from './seconds'
  import minuteCom from './minute'
  import hourCom from './hour'
  import dayCom from './day'
  import monthCom from './month'
  import weekCom from './week'
  import yearCom from './year'

  export default {
    name: 'CzrVueCron',
    components: { secondsCom, minuteCom, hourCom, dayCom, monthCom, weekCom, yearCom },
    props: {
      cron: {}
    },
    data() {
      return {
        mode: 1,
        secondsVal: '',
        minuteVal: '',
        hourVal: '',
        dayVal: '',
        monthVal: '',
        weekVal: '',
        yearVal: '',
      }
    },
    computed: {
      cronVal() {
        return `${this.secondsVal} ${this.minuteVal} ${this.hourVal} ${this.dayVal} ${this.monthVal} ${this.weekVal}${this.yearVal ? ` ${this.yearVal}` : ''}`
      }
    },
    watch: {
      cronVal(n) {
        this.$emit('update:cron', n)
      }
    },
    mounted() {
      this.initVal(this.cron)
    },
    methods: {
      initVal() {
        if (this.cron) {
          const cronNodes = this.cron.split(' ')
          if (cronNodes[0]) {
            this.secondsVal = cronNodes[0]
          }
          if (cronNodes[1]) {
            this.minuteVal = cronNodes[1]
          }
          if (cronNodes[2]) {
            this.hourVal = cronNodes[2]
          }
          if (cronNodes[3]) {
            this.dayVal = cronNodes[3]
          }
          if (cronNodes[4]) {
            this.monthVal = cronNodes[4]
          }
          if (cronNodes[5]) {
            this.weekVal = cronNodes[5]
          }
          if (cronNodes[6]) {
            this.yearVal = cronNodes[6]
          } else {
            this.yearVal = ''
          }
        } else {
          this.secondsVal = '*'
          this.minuteVal = '*'
          this.hourVal = '*'
          this.dayVal = '*'
          this.monthVal = '*'
          this.weekVal = '?'
          this.year = ''
        }
      }
    }
  }
</script>

<style lang="scss" scoped>
  $tabsBackgroundColor: #F2F2F2;
  $tabBackgroundColor: #a6cbed;
  .radio {

  }
  .cron {
    width: 100%;
    height: 100%;
    .button {
      display: flex;
      justify-content: center;
      align-items: center;
      width: 180px;
      height: 38px;
      background-color: #1890ff;
      font-size: 18px;
      color: #ffffff;
      border-radius: 2px;
      cursor: pointer;
    }
    .tabs {
      height: 30px;
      display: flex;
      align-items: center;
      background-color: $tabsBackgroundColor;
      .tab {
        width: 50px;
        height: 100%;
        display: flex;
        align-items: center;
        justify-content: center;
        cursor: pointer;
        &.active {
          background-color: $tabBackgroundColor;
        }
        &:hover {
          background-color: $tabBackgroundColor;
          opacity: 0.9;
        }
      }
    }
  }
</style>
