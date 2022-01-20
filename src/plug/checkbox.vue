<template>
  <div class="checkbox">
    <div class="item" :class="{active: item.active}" v-for="item in nodes">
      <input type="checkbox" :checked="item.active" @change="handleVal(item)"/>{{item.label}}
    </div>
  </div>
</template>

<script>
  export default {
    props: {
      nodes: {},
      list: {}
    },
    methods: {
      handleVal(v) {
        v.active = !v.active
        const temp = this.nodes.filter(v => {
          return v.active
        }).map(v => {
          return v.val
        })
        this.$emit('update:list', temp)
      }
    }
  }
</script>

<style lang="scss" scoped>
  .checkbox {
    width: 100%;
    height: 100%;
    display: flex;
    flex-wrap: wrap;
    .item {
      min-width: 40px;
      display: flex;
      align-items: center;
      justify-content: space-between;
      margin-right: 14px;
      margin-bottom: 6px;
      padding: 0 4px;
      border-radius: 4px;
      &.active {
        border: 1px solid #0075FF;
      }
    }
  }
</style>
