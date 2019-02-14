<template>
    <div id="wrapper3" class="wrap">
      <div id="lev" class="chart"></div>
      <h3>{{name}}</h3>
    </div>

</template>

<script>
  import echarts from 'echarts';
  export default{
    props:['data'],
      data(){
       return {
         name:'水箱液位',
         myChart:'',
         option:{},
         waterLev:'',
       }
      },
    created(){
      this.$watch("data", function (newValue, oldValue) {
        this.waterLev = this.option.series[0].data[0] = this.data.waterLev
        this.option.yAxis[0].axisLine.lineStyle.color = this.waterLev>3?'red':'#42B8FA'
        this.myChart.setOption(this.option, true)
      })
    },
    mounted(){
      this.myChart = echarts.init(document.getElementById('lev'))
      this.showData()
      this.waterLev = this.option.series[0].data[0] = this.data.waterLev
      this.myChart.setOption(this.option, true)
    },
    methods:{
      showData(){
          this.option = {
           textStyle:{
             color:'#fff',
             fontSize:18
           },
            grid: {
             bottom:'-1',
              containLabel: true
            },
            xAxis : [
              {
                type : 'category',
                show:true
              }
            ],
            yAxis : [
              {
                type : 'value',
                max:5,
                min:1,
                offset:'8',
                splitNumber:4,
                axisLine:{
                  show:true,
                  lineStyle:{
                    color:this.waterLev>3?'red':'#42B8FA',
                    width:3,
                  }
                },
                axisTick:{
                  show:true,
                  inside:true
                },
                splitLine:{//网格线
                  show:false
                },
                axisLabel:{
                  // formatter: function (value) {
                  //   var texts = [];
                  //   if(value==1){
                  //     texts.push('1m');
                  //   }
                  //   else if (value <=2) {
                  //     texts.push('2m');
                  //   }
                  //   else if (value<= 3) {
                  //     texts.push('3m');
                  //   }
                  //   else if(value<= 4){
                  //     texts.push('4m');
                  //   }
                  //   return texts;
                  // },
                  formatter:'{value}m',
                  fontSize:18,
                }
              }
            ],
            series : [
              {
                type:'bar',
                itemStyle: {
                  normal: {
                    color:'#42B8FA',
                    barBorderRadius: 10,
                  }
                },
                barWidth :145,
                z: 10,
                barGap: '-100%', // Make series be overlap
                data:[(this.waterLev)],
                label:{
                   show:true,
                    position:'top',
                    color:'#fff',
                    fontSize:30,
                  fontWeight:'bold'
                }
              },
              {
                type: 'bar',
                itemStyle: {
                  normal: {
                    color: '#999',
                    barBorderRadius: 10,
                    shadowColor : '#fff',
                    shadowBlur: 5
                  },
                },
                barWidth: 145,
                data: [5]
              },
            ]
          }
          if (this.option && typeof this.option === "object") {
           this.myChart.setOption(this.option, true);
          }
        },
    }
  }
</script>

<style scoped>
  #wrapper3{
    position:relative;
    text-align:center;
    margin-top:-50px;
  }
  #wrapper3  .chart{
    margin-top:20px;
  }
  #wrapper3 h3{
    margin-top:50px;
  }
</style>
