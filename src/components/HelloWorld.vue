<template>
  <div class="hello">
    <h1>{{ msg }}</h1>
    <button @click="toLogin">登录</button>

    <div class="container">
      <div class="row">
        <div class="col-md-3"></div>
        <div class="col-md-3 offset-6"></div>
      </div>
    </div>

    <div class="container-fluid">
    </div>

  </div>
</template>

<script>
import 'bootstrap/dist/css/bootstrap.min.css' 
import 'bootstrap/dist/js/bootstrap.min'

//测试jquery是否引入成功
$(function () {
 alert('jquery is ready22222!!!!')  
})

export default {
  name: 'HelloWorld',
  data () {
    return {
      msg: '哈哈，这是一个使用bootstrap的vue项目'
    }
  },
  methods:{
    toLogin:function(){
      this.$router.push("/login")
    }
  }
}
</script>

<!-- Add "scoped" attribute to limit CSS to this component only -->
<style scoped>
h1, h2 {
  font-weight: normal;
}
ul {
  list-style-type: none;
  padding: 0;
}
li {
  display: inline-block;
  margin: 0 10px;
}
a {
  color: #42b983;
}

.container>.row{
  background: red;
}
.container>.row>div{
  height: 100px;
  background: pink;
  border: 1px solid #ccc
}

.container-fluid{
  height: 100px;
  background: purple;
}
</style>
