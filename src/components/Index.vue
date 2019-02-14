<template>
    <div id="Index">
      <div v-if="flag" id="have">
        <Odata num="1" name="市政进水压力" :data="data"  min="0" max="1.6" floor="0.4" upper="1.2" ></Odata>
        <Tdata :data="data" ></Tdata>
        <Odata num="2" name="恒压泵出水压力" :data="data" min="0" max="1.6" floor="0.4" upper="1.2" ></Odata>
      </div>
      <div v-else id="center">数据请求中...</div>
    </div>
</template>
<script>
  import Odata from '@/components/oData'
  import Tdata from '@/components/tData'

  export default {
    data(){
      return {
        data:{},
        flag:false
      }
    },
    components:{
      Odata,
      Tdata,
    },
    mounted(){
      this.getValue()
      setInterval( ()=> {
       this.getValue()
      },10000)
    },
    methods:{
      getValue() {
        this.$.get('../../static/shipa.json',(result) => {
          var random = Math.floor(Math.random()*10);
          this.data = result.list[random];
          this.flag = true;
        })
      }
    }
  }
</script>

<style>
  #Index{
    height:80%;
    width:100%;
    overflow:hidden;
    padding:0 50px;
    box-sizing:border-box;
    text-align:center;
    display:flex;
    justify-content: center;
    align-items:center;
    margin-top:10px;
  }
  #have{
    display:flex;
    justify-content: space-between;
    align-items:center;
    height:100%;
    width:100%;
    overflow:hidden;
    margin-top:-30px;
  }
  #wrapper1,#wrapper2{
    width:36%;
    text-align:center;
  }
  #wrapper3{
    width:25%;
    text-align:center;
  }
  .chart{
    width:100%;
    height:330px;
    border-radius:50%;
    margin:0 auto;
  }
   .wrap>h3{
    font-size:30px;
    margin-top:20px;
  }
</style>
