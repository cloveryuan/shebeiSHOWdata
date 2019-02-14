<template>
  <div id="app">
    <h1>中央空调水系统监测</h1>
    <router-view/>
    <em class="mark">南京紫图科技有限公司承制</em>
  </div>
</template>

<script>
export default {
  name: 'App'
}
</script>

<style>
  *{
    margin:0;
    padding:0
  }
  body,html{
    height:100%;
    background:#262B2E;
    color:#fff;
  }
#app{
  height:100%;
  width:100%;
  overflow:hidden;
}
  #app>h1{
    text-align:center;
    margin-top:30px;
    font-size:28px;
  }
  .mark{
    color:#fff;
    position:absolute;
    right:50px;
    font-size:18px;

  }
</style>
