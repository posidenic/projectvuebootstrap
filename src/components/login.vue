<template>
  <div class="login">
    <div class="login_box">
      <ul class="login_ul">
        <li>
          <el-input placeholder="请输入手机号" v-model="userPhone" value="userPhone" maxlength="11">
            <template slot="prepend">手机号</template>
          </el-input>
        </li>
        <li>
          <el-input placeholder="请输入密码" v-model="password" type="password" minlength="6">
            <template slot="prepend">密&nbsp;&nbsp;&nbsp;码</template>
          </el-input>
        </li>
        <li>
          <el-button type="primary" @click="login()" :loading="loading" style="width:100%;">登录</el-button>
        </li>
        <li>
          <el-checkbox v-model="isPwd">记住手机号和密码</el-checkbox>
          <span class="el-checkbox__label" style="float:right;margin-top:1px;" @click="gotopage('resetpwd')">忘记密码</span>
          <!--<a href="javaScript:void(0)" @click="gotopage('resetpwd')">忘记密码</a>-->
          <!--<a href="javaScript:void(0)" @click="gotopage('regist')">注册</a>-->
        </li>
      </ul>
    </div>
  </div>
</template>
<script>
  export default {
    data() {
      return {
        userPhone: '',
        password: '',
        isPwd: false,
        loading: false
      }
    },
    created(){
      let pwd=JSON.parse(localStorage.getItem('user'));
      if(pwd){
        this.isPwd=true;
        this.userPhone=pwd.account;
        this.password=pwd.password;
      }
    },
    methods: {
      login() {
        let self = this;
        self.url = self.$interfaces.login.login;
        let patt = /^1\d{10}$/g;
        if (self.userPhone == '' || !patt.test(self.userPhone)) {
          self.$message.error('请检查手机号');
          return;
        } else if (self.password == '') {
          self.$message.error('请输入密码');
          return;
        } else if (self.password.length < 6) {
          self.$message.error('密码最少六位');
          return;
        }
        self.loading = true;
        let params = {
          account: this.userPhone,
          password: this.password,
        };

        this.$axios.post(self.url, params).then((res) => {
          self.loading = false;
          if (res.data.state.stateCode == 0) {
            // sessionStorage.setItem('username', res.data.data.name);
            sessionStorage.setItem('username', self.userPhone);
            sessionStorage.setItem('userInfo', 'userInfo');
            self.$router.push({name: "carList"});
            if(self.isPwd){
              localStorage.setItem('user',JSON.stringify(params));
            }
          } else {
            if (res.data.state.stateMessage == "") {
              self.$message.error('登录失败：未知原因')
            } else {
              self.$message.error(res.data.state.stateMessage)
            }
          }

        }).catch((error) => {
          this.loading = false;
          this.$handleError(error, this);
        });
      },
      gotopage(key) {
        var self = this;
        self.$router.push({name: key})
      }
    }
  }
</script>
<style>
  /*whitesmoke*/
  .el-checkbox__label {
    color: whitesmoke;
  }

  .login {
    width: 100%;
    height: 100%;
    background: #3A3A3A;
  }

  .login_box {
    min-width: 240px;
    width: 18%;
    max-width: 300px;
    background: #3a3a3a;
    position: absolute;
    top: 50%;
    left: 50%;
    -webkit-transform: translate(-50%, -50%);
    -moz-transform: translate(-50%, -50%);
    transform: translate(-50%, -50%);
  }

  .login_box img {
    width: 74%;
    min-width: 200px;
    display: block;
    margin: 10px auto;
  }

  .login_box .login_ul li {
    margin-bottom: 16px;
  }

  .login_box .login_ul li a {
    float: right;
    margin-left: 10px;
  }
</style>
