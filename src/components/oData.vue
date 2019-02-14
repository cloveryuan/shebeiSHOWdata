<template>
    <div :id="'wrapper'+ num" class="wrap">
        <div :id="'chart'+ num " class="chart"></div>
        <h3>{{name}}</h3>
    </div>
</template>
<script>
  import echarts from 'echarts';
  export default {
    props:['num','name','data','min','max','floor','upper'],
    data() {
      return {
        myChart:'',
        option:{},
        color:'',
        init:''
      }
    },
    created(){
      this.$watch("data", function (newValue, oldValue) {
        this.init = this.option.series[0].data[0].value = (this.name=="市政进水压力")? (this.data.sp):(this.data.hp)
        this.color =this.option.series[0].detail.backgroundColor =  this.init<=this.floor?'red':this.init>=this.upper?'#ff4500':'#08FF08'
        this.myChart.setOption(this.option);
        // console.log(newValue)
      })
    },
    mounted() {
      this.drawLine()
      this.init = this.option.series[0].data[0].value = (this.name=="市政进水压力"? (this.data.sp):(this.data.hp))
      this.color =this.option.series[0].detail.backgroundColor =  this.init<= this.floor?'red':this.init>=this.upper?'#ff4500':'#08FF08'
      this.myChart.setOption(this.option);
    },
    methods: {
      drawLine() {
        // 基于准备好的dom，初始化echarts实例
        this.myChart = echarts.init(document.getElementById("chart" + this.num))
        // 绘制图表
        this.option = {
          tooltip: {
            formatter: "{a} <br/>{c} {b}"
          },
          toolbox: {
            show: true,
          },
          series: [
            {
              name: '压力',
              type: 'gauge',
              // startAngle: 225,
              // endAngle: -45,
              splitNumber: 4,
              min: this.min,
              max: this.max,
              precision: 1,
              radius: '85%',
              axisLine: {            // 坐标轴线
                lineStyle: {       // 属性lineStyle控制线条样式
                  color: [[(this.floor/this.max).toFixed(2), 'red'], [(this.upper/this.max).toFixed(2), '#08ff08'], [1, '#ff4500']],
                  width: 5,
                  shadowColor: '#fff', //默认透明
                  shadowBlur: 10
                }
              },
              axisLabel: {            // 坐标轴文字标记
                textStyle: {       // 属性textStyle控制坐标轴文字样式
                  fontWeight: 'bolder',
                  color: '#fff',
                  fontSize:24,
                  shadowColor: '#fff', //默认透明
                }
              },

              axisTick: {
                show: true,// 坐标轴分割线
                length: 15,        // 属性length控制线长
                splitNumber: 5,
                lineStyle: {       // 属性lineStyle控制线条样式
                  color: 'auto',
                  shadowColor: '#fff', //默认透明
                  shadowBlur: 10
                }
              },
              splitLine: {           // 段块分隔线
                length: 20,         // 属性length控制线长
                lineStyle: {
                  width: 1.5,
                  color: '#fff',
                  shadowColor: '#fff', //默认透明
                  shadowBlur: 10
                }
              },
              pointer: {           // 仪表盘指针长宽
                width: 8,
                length: '70%'
              },
              title: {//仪表盘标题
                fontWeight: 'bolder',
                fontSize: 22,
                fontStyle: 'italic',
                color: '#fff',
                shadowColor: '#fff', //默认透明
                shadowBlur: 10
              },
              detail: {//仪表盘详情，用于显示数据。
                backgroundColor:'lime',
                borderWidth: 1,
                borderColor: '#fff',
                shadowColor: '#fff', //默认透明
                shadowBlur: 5,
                offsetCenter: [0, '90%'],       // x, y，单位px
                fontWeight: 'bolder',
                color: '#fff',
                borderRadius: 6,
                fontSize: 40,
                formatter: function (value) {
                  return value + 'Mpa'
                }
              },
              data: [{value:this.init, name: 'Mpa'}],
              markPoint:{
                symbol:'circle',
                symbolSize:8,
                data:[
                    //跟你的仪表盘的中心位置对应上，颜色可以和画板底色一样
                  {x:'center',y:'center',itemStyle:{color:'rgba(0,0,0,0.7)'}}
                ]
              }
            }
          ]
        }
        if (this.option && typeof this.option === "object") {
          this.myChart.setOption(this.option, true);
        }
      }
    }
  }
</script>

<style scoped>

</style>
