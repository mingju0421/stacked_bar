<template>
  <div class="hello">
    <StackedBar class="stackedBar" :data='data'/>
  </div>
</template>

<script>
import StackedBar from './echarts/stackedBar'
export default {
  name: 'HelloWorld',
  components: {
    StackedBar,
  },
  data () {
    return {
      data: [
        ['直接访问', [['周一',  320],['周二',  302], ['周三',  301], ['周四',  334], ['周五',  390], ['周六',  330],  ['周日',  320]]],
        ['邮件营销', [['周一',  120],['周二',  132], ['周三',  101], ['周四',  134], ['周五',  90], ['周六',  230],  ['周日',  210]]], 
        ['联盟广告', [['周一', 220],['周二',  182], ['周三',  191], ['周四',  234], ['周五',  290], ['周六',  330],  ['周日',  310]]], 
        ['视频广告', [['周一',  150],['周二', 212], ['周三',  201], ['周四',  154], ['周五',  190], ['周六',  330],  ['周日',  410]]],
        ['搜索引擎', [['周一',  820],['周二', 832], ['周三',  901], ['周四',  934], ['周五',  1290], ['周六',  1330],  ['周日',  1320]]], 
      ]
    }
  },
  mounted () {
    setTimeout(() => {
      this.data[0].splice(1, 1, [['周一',  320],['周二',  0], ['周三',  0], ['周四',  0], ['周五',  0], ['周六',  0],  ['周日',  320]])
    },2000)
  },
  methods: {
    xxx (arr) {
      let arr1 = JSON.stringify(arr)
      let arr2 = JSON.parse(arr1)
      let k = 0
      while (k < 7) {
        let sum = 0
        for (let i = 0; i < arr2.length; i++) {
          sum += arr2[i][1][k][1]
        }
        for (let j = 0; j < arr2.length; j++) {
          arr2[j][1][k].splice(1,0, Math.floor(arr2[j][1][k][1] / sum * 10000) / 100)
        }
        k++
      }
      this.data1 = arr2
    }
  }
}
</script>

<style lang="stylus" scoped>
.stackedBar{
  height 500px
}
</style>
