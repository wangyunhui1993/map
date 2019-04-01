<template>
  <div id="app">
    <router-view/>
  </div>
</template>

<script>
export default {
  name: 'App'
}
</script>

<style>
body {
  padding:0px;
  margin:0px auto;
  height: 100%;
}
*{
  margin: 0;
  padding: 0;
}
a{
  text-decoration:none;
}
.clearfix:after{
  content:'';
  display: block;
  clear:both;
  height:0;
  visibility: hidden;
}
#app {
  font-family: 'Avenir', Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  position: absolute;
  top: 0;
  bottom: 0;
  width: 100%;
  height:100%;
  /*text-align: center;*/
  color: #2c3e50;
  /*margin-top: 60px;*/
}
.pagination {
  float: right;
  margin: 20px 0;
}
.operate-btn {
  margin-bottom: 10px;
}
.el-dropdown-menu__item {
  padding: 0;
}
</style>
