<template>
  <div class="year">
    允许的通配符[, - * /]，非必填
    <template v-if="mode === 9">
      <span style="color: #f5222d">表达式反解析有误</span>
    </template>
    <div class="item">
      <div class="radio" :class="{active: mode === 1}" @click="mode = 1">
        <div class="r-block"/>
      </div>
      <div class="content">
        不指定
      </div>
    </div>
    <div class="item">
      <div class="radio" :class="{active: mode === 2}" @click="mode = 2">
        <div class="r-block"/>
      </div>
      <div class="content">
        每年，允许的通配符[, - * /]
      </div>
    </div>
    <div class="item">
      <div class="radio" :class="{active: mode === 3}" @click="mode = 3">
        <div class="r-block"/>
      </div>
      <div class="content">
        周期从
        <input v-model="cycleVal_1" type="number" :min="year" max="2099" step="1"/>
        年-
        <input v-model="cycleVal_2" type="number" :min="year" max="2099" step="1"/>
        年
      </div>
    </div>
  </div>
</template>

<script>

  export default {
    props: {
      val: {}
    },
    data() {
      const year = new Date().getFullYear()
      return {
        year: year,
        mode: 1,
        cycleVal_1: year,
        cycleVal_2: year,
      }
    },
    computed: {
      cronVal() {
        let val = ''
        if (this.mode === 1) {
          val = ''
        } else if (this.mode === 2) {
          val = '*'
        } else if (this.mode === 3) {
          val = `${this.cycleVal_1}-${this.cycleVal_2}`
        }
        return val
      }
    },
    watch: {
      val(n) {
        if (n === '') {
          this.mode = 1
        } else if (n === '*') {
          this.mode = 2
        } else if (n.includes('-')) {
          const temp = n.split('-')
          this.cycleVal_1 = temp[0]
          this.cycleVal_2 = temp[1]
          this.mode = 3
        } else {
          this.mode = 9
        }
      },
      cronVal(n) {
        this.$emit('update:val', n)
      },
      cycleVal_1(n, o) {
        if (n === 'e') {
          this.cycleVal_1 = o
        } else {
          this.cycleVal_1 = this.format(this.year, 2099, n)
        }
      },
      cycleVal_2(n, o) {
        if (n === 'e') {
          this.cycleVal_2 = o
        } else {
          this.cycleVal_2 = this.format(this.year, 2099, n)
        }
      }
    },
    methods: {
      format(start, end, val) {
        const numVal = Number(val)
        if (numVal < start) {
          return start
        } else if (numVal > end) {
          return end
        } else {
          return Math.floor(numVal)
        }
      }
    }
  }
</script>

<style lang="scss" scoped>
  $radioBackgroundColor: #0075FF;
  .year {
    .item {
      display: flex;
      align-items: center;
      .radio {
        width: 16px;
        height: 16px;
        border-radius: 50%;
        border: 1px solid #000000;
        position: relative;
        display: flex;
        align-items: center;
        justify-content: center;
        cursor: pointer;
        margin-right: 6px;
        &.active {
          .r-block {
            background-color: $radioBackgroundColor;
          }
        }
        .r-block {
          position: absolute;
          width: 10px;
          height: 10px;
          border-radius: 50%;
        }
      }
    }
  }
</style>
