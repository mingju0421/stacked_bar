<template>
  <div>
    <div class="echartsWrap"  ref="echartsWrap"/>
  </div>
</template>

<script>
import echarts from 'echarts';
export default {
  name: 'stackedBar',
  props: ['data'],
  data () {
    return {
      stackedBar: null,
      option: {
        tooltip : {
            trigger: 'axis',
            axisPointer : {            
                type : 'shadow'        
            },
            formatter: function (params,){
              let str = ''
              for (let i = 0; i < params.length-1; i++) { 
                str += `<span style="color:${params[i].color}">${params[i].seriesName}:  ${params[i].value[2]}</span> <br/>`
              }
              return  str
            }
        },
        legend: {
          data: ['直接访问', '邮件营销','联盟广告','视频广告','搜索引擎']
        },
      
        yAxis:  {
          type: 'value',
          axisLabel: {
                  show: true,
                  interval: 'auto',
                  formatter: '{value} %'
              },
          show: true
        },
        xAxis: {
          type: 'category',
          data: ['周一','周二','周三','周四','周五','周六','周日']
        },
        series: [
          
          
        ]
      },
      seriesSum: {
        name: '合计',
        type: 'bar',
        stack: '总量',
        label: {
          normal: {
            show: true,
            position: 'top',
            formatter: (val) => {
              let sum = 0;
              let series = this.option.series;
              if (series.length < 2) return
              for(var i=0,l=series.length;i<l;i++){ 
                sum += series[i].data[val.dataIndex][2]
              } 
              return sum
            }
          }
        },
        data:  [['周一', 0, 0],['周二',0, 0], ['周三', 0, 0], ['周四', 0, 0], ['周五', 0, 0], ['周六', 0, 0],  ['周日', 0, 0]]
      }
    }
  },
  methods: {
    update (arr, that) {
      // if (arr.length < 1) return
      arr = this.xxx(arr)
      let seriesSum = that.seriesSum;
      let series = [];
      for (let i = 0; i < arr.length; i++) {
        let seriesItem = {
          type: 'bar',
          stack: '总量',
          label: {
            normal: {
              show: true,
              position: 'insideRight',
              formatter: (c) => {
                // console.log(c)
                if(c.data[2]) {
                  return `${c.data[1]}%`
                }else {
                  return ''
                }
              }
            }
          },
        }
        series.push(seriesItem);
        series[i].name = arr[i][0]
        series[i].data = arr[i][1]
      }
      
      series.push(seriesSum)
      console.log(series)
      console.log(arr.length)
      that.option.series = series
      this.stackedBar.setOption(that.option)
    },
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
      return arr2
    }
  },
  mounted () {
    this.stackedBar = echarts.init(this.$refs.echartsWrap)
    this.update(this.data, this)
  },
  watch: {
    data () {
      this.update(this.data, this)
    }
  }
}
</script>

<style lang="stylus" scoped>
.echartsWrap{
  height 100%
}
</style>