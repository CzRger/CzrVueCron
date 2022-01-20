# czr-vue-cron

npm install czr-vue-cron

![Image text](https://gitee.com/CzRger/CzrVueCron/raw/master/src/assets/img_1.png)

## 引入
```vue
<template>
  <div id="app">
    cron表达式：<input v-model="cronVal"/>
    <CzrVueCron :cron.sync="cronVal"/>
  </div>
</template>

<script>
import CzrVueCron from 'czr-vue-cron'

export default {
  components: {
    CzrVueCron
  },
  data() {
    return {
      cronVal: ''
    }
  }
}
</script>
```
