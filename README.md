# czr-vue-cron

npm install czr-vue-cron

![Image text](https://gitee.com/CzRger/CzrVueCron/raw/master/src/assets/img_1.png)

## 引入
```vue
<template>
  <div id="app">
    cron表达式：<input v-model="cronVal"/>
    <CzrVueCron :cron.sync="cronVal" :recent="[5, 5]"/>
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
## 参数
```
cron：表达式参数
recent(非必传)：
    [last, will]
    last-过去运行次数
    will-未来运行次数
```
## 问题
```
如有问题请联系：526948392@qq.com
```
