<template>
  <div class="day">
    <template v-if="mode === 9">
      <span style="color: #f5222d">表达式反解析有误</span>
    </template>
    <div style="text-align: left">允许的通配符[,- * ? / L W C]</div>
    <div class="item">
      <div class="radio" :class="{active: mode === 1}" @click="mode = 1">
        <div class="r-block"/>
      </div>
      <div class="content">
        每日
      </div>
    </div>
    <div class="item">
      <div class="radio" :class="{active: mode === 2}" @click="mode = 2">
        <div class="r-block"/>
      </div>
      <div class="content">
        不指定
      </div>
    </div>
    <div class="item">
      <div class="radio" :class="{active: mode === 3}" @click="mode = 3">
        <div class="r-block"/>
      </div>
      <div class="content">
        周期从
        <input v-model="cycleVal_1" type="number" min="1" max="31" step="1"/>
        -
        <input v-model="cycleVal_2" type="number" min="1" max="31" step="1"/>
        日
      </div>
    </div>
    <div class="item">
      <div class="radio" :class="{active: mode === 4}" @click="mode = 4">
        <div class="r-block"/>
      </div>
      <div class="content">
        从
        <input v-model="toVal_1" type="number" min="1" max="31" step="1"/>
        日开始，每
        <input v-model="toVal_2" type="number" min="1" max="31" step="1"/>
        天执行一次
      </div>
    </div>
    <div class="item">
      <div class="radio" :class="{active: mode === 5}" @click="mode = 5">
        <div class="r-block"/>
      </div>
      <div class="content">
        每月
        <input v-model="everyVal" type="number" min="1" max="31" step="1"/>
        日最近的那个工作日
      </div>
    </div>
    <div class="item">
      <div class="radio" :class="{active: mode === 6}" @click="mode = 6">
        <div class="r-block"/>
      </div>
      <div class="content">
        本月最后一天
      </div>
    </div>
    <div class="item">
      <div class="radio" :class="{active: mode === 7}" @click="handleDesign">
        <div class="r-block"/>
      </div>
      <div class="content">
        指定
      </div>
    </div>
    <div class="design">
      <CheckBoxCom :nodes.sync="designNodes" :list.sync="designList"/>
    </div>
  </div>
</template>

<script>
  import CheckBoxCom from './checkbox'

  export default {
    components: { CheckBoxCom },
    props: {
      val: {}
    },
    data() {
      return {
        mode: 1,
        cycleVal_1: 1,
        cycleVal_2: 1,
        toVal_1: 1,
        toVal_2: 1,
        everyVal: 1,
        designNodes: [
          {val: 1, label: 1, active: false},
          {val: 2, label: 2, active: false},
          {val: 3, label: 3, active: false},
          {val: 4, label: 4, active: false},
          {val: 5, label: 5, active: false},
          {val: 6, label: 6, active: false},
          {val: 7, label: 7, active: false},
          {val: 8, label: 8, active: false},
          {val: 9, label: 9, active: false},
          {val: 10, label: 10, active: false},
          {val: 11, label: 11, active: false},
          {val: 12, label: 12, active: false},
          {val: 13, label: 13, active: false},
          {val: 14, label: 14, active: false},
          {val: 15, label: 15, active: false},
          {val: 16, label: 16, active: false},
          {val: 17, label: 17, active: false},
          {val: 18, label: 18, active: false},
          {val: 19, label: 19, active: false},
          {val: 20, label: 20, active: false},
          {val: 21, label: 21, active: false},
          {val: 22, label: 22, active: false},
          {val: 23, label: 23, active: false},
          {val: 24, label: 24, active: false},
          {val: 25, label: 25, active: false},
          {val: 26, label: 26, active: false},
          {val: 27, label: 27, active: false},
          {val: 28, label: 28, active: false},
          {val: 29, label: 29, active: false},
          {val: 30, label: 30, active: false},
          {val: 31, label: 31, active: false},
        ],
        designList: []
      }
    },
    computed: {
      cronVal() {
        let val = ''
        if (this.mode === 1) {
          val = '*'
        } else if (this.mode === 2) {
          val = '?'
        } else if (this.mode === 3) {
          val = `${this.cycleVal_1}-${this.cycleVal_2}`
        } else if (this.mode === 4) {
          val = `${this.toVal_1}/${this.toVal_2}`
        } else if (this.mode === 5) {
          val = `${this.everyVal}W`
        } else if (this.mode === 6) {
          val = 'L'
        } else if (this.mode === 7) {
          val = this.designList.length === 0 ? '?' : this.designList.join(',')
        }
        return val
      }
    },
    watch: {
      val(n) {
        if (n === '*') {
          this.mode = 1
          this.$emit('reset', '?')
        } else if (n === '?') {
          this.mode = 2
          if (this.$parent.mode === 4) {
            this.$emit('reset', '*')
          }
        } else if (n.includes('-')) {
          const temp = n.split('-')
          this.cycleVal_1 = temp[0]
          this.cycleVal_2 = temp[1]
          this.mode = 3
          this.$emit('reset', '?')
        } else if (n.includes('/')) {
          const temp = n.split('/')
          this.toVal_1 = temp[0]
          this.toVal_2 = temp[1]
          this.mode = 4
          this.$emit('reset', '?')
        } else if (n.includes('W')) {
          const temp = n.split('W')
          this.everyVal = temp[0]
          this.mode = 5
          this.$emit('reset', '?')
        } else if (n === 'L') {
          this.mode = 6
          this.$emit('reset', '?')
        } else if (n.includes(',') || new RegExp(/(^([1-2][0-9]{1})$)|(^[1-9]{1}$)|(^[3][0-1]{1}$)/).test(n)) {
          const temp = n.split(',')
          this.designNodes.forEach(v => v.active = false)
          temp.forEach(v => {
            this.designNodes[Number(v) - 1].active = true
          })
          this.designList = temp
          this.mode = 7
          this.$emit('reset', '?')
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
          this.cycleVal_1 = this.format(1, 31, n)
        }
      },
      cycleVal_2(n, o) {
        if (n === 'e') {
          this.cycleVal_2 = o
        } else {
          this.cycleVal_2 = this.format(1, 31, n)
        }
      },
      toVal_1(n, o) {
        if (n === 'e') {
          this.toVal_1 = o
        } else {
          this.toVal_1 = this.format(1, 31, n)
        }
      },
      toVal_2(n, o) {
        if (n === 'e') {
          this.toVal_2 = o
        } else {
          this.toVal_2 = this.format(1, 31, n)
        }
      },
      everyVal(n, o) {
        if (n === 'e') {
          this.everyVal = o
        } else {
          this.everyVal = this.format(1, 31, n)
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
      },
      handleDesign() {
        if (this.designList.length === 0) {
          this.designNodes[0].active = true
          this.designList[0] = this.designNodes[0].val
        }
        this.mode = 7
      }
    }
  }
</script>

<style lang="scss" scoped>
  $radioBackgroundColor: #0075FF;
  .day {
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
