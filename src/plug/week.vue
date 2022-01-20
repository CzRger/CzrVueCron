<template>
  <div class="week">
    <div>允许的通配符[, - * ? / L C #]</div>
    <template v-if="mode === 9">
      <span style="color: #f5222d">表达式反解析有误</span>
    </template>
    <div class="item">
      <div class="radio" :class="{active: mode === 1}" @click="mode = 1">
        <div class="r-block"/>
      </div>
      <div class="content">
        每周
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
        周期从星期
        <input v-model="cycleVal_1" type="number" min="1" max="7" step="1"/>
        -星期
        <input v-model="cycleVal_2" type="number" min="1" max="7" step="1"/>
      </div>
    </div>
    <div class="item">
      <div class="radio" :class="{active: mode === 4}" @click="mode = 4">
        <div class="r-block"/>
      </div>
      <div class="content">
        第
        <input v-model="specialVal_1" type="number" min="1" max="4" step="1"/>
        周的星期
        <input v-model="specialVal_2" type="number" min="1" max="7" step="1"/>
      </div>
    </div>
    <div class="item">
      <div class="radio" :class="{active: mode === 5}" @click="mode = 5">
        <div class="r-block"/>
      </div>
      <div class="content">
        本月最后一个星期
        <input v-model="lastVal" type="number" min="1" max="7" step="1"/>
      </div>
    </div>
    <div class="item">
      <div class="radio" :class="{active: mode === 6}" @click="handleDesign">
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
        specialVal_1: 1,
        specialVal_2: 1,
        lastVal: 1,
        designNodes: [
          {val: 1, label: '1(周日)', active: false},
          {val: 2, label: '2(周一)', active: false},
          {val: 3, label: '3(周二)', active: false},
          {val: 4, label: '4(周三)', active: false},
          {val: 5, label: '5(周四)', active: false},
          {val: 6, label: '6(周五)', active: false},
          {val: 7, label: '7(周六)', active: false},
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
          val = `${this.specialVal_1}/${this.specialVal_2}`
        } else if (this.mode === 5) {
          val = `${this.lastVal}L`
        } else if (this.mode === 6) {
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
          if (this.$parent.mode === 6) {
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
          this.specialVal_1 = temp[0]
          this.specialVal_2 = temp[1]
          this.mode = 4
          this.$emit('reset', '?')
        } else if (n.includes('L')) {
          const temp = n.split('L')
          this.lastVal = temp[0]
          this.mode = 5
          this.$emit('reset', '?')
        } else if (n.includes(',') || new RegExp(/^[1-7]{1}$/).test(n)) {
          const temp = n.split(',')
          temp.forEach(v => {
            this.designNodes[Number(v) - 1].active = true
          })
          this.designList = temp
          this.mode = 6
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
          this.cycleVal_1 = this.format(1, 7, n)
        }
      },
      cycleVal_2(n, o) {
        if (n === 'e') {
          this.cycleVal_2 = o
        } else {
          this.cycleVal_2 = this.format(1, 7, n)
        }
      },
      specialVal_1(n, o) {
        if (n === 'e') {
          this.specialVal_1 = o
        } else {
          this.specialVal_1 = this.format(1, 4, n)
        }
      },
      specialVal_2(n, o) {
        if (n === 'e') {
          this.specialVal_2 = o
        } else {
          this.specialVal_2 = this.format(1, 7, n)
        }
      },
      lastVal(n, o) {
        if (n === 'e') {
          this.lastVal = o
        } else {
          this.lastVal = this.format(1, 7, n)
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
        this.mode = 6
      }
    }
  }
</script>

<style lang="scss" scoped>
  $radioBackgroundColor: #0075FF;
  .week {
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
