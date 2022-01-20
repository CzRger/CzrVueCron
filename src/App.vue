<template>
  <div id="app">
    Cron表达式: <input v-model="cronVal"/>
    <CzrCron :cron.sync="cronVal"/>
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
