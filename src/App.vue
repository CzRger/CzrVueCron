<template>
  <div id="app">
    Cron表达式: <input v-model="cronVal"/>
    <CzrCron :cron.sync="cronVal" :recent="[5, 5]"/>
  </div>
</template>

<script>
  import CzrCron from './plug/index.vue'

  export default {
    name: 'App',
    components: {
      CzrCron
    },
    data() {
      return {
        cronVal: ''
      }
    }
  }
</script>

<style lang="scss">

</style>
