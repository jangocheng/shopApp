<template>
    <div>
        <div class="box show">
            <p><input type="text" @focus="clear('phone')" :class="{red:phoneRed}" v-model.trim="phone" placeholder="请输入手机号"><img src="../../../assets/images/icon1.jpg"></p>
            <p><input type="password" @focus="clear('pwd')" :class="{red:pwdRed}" v-model="pwd" placeholder="请输入密码"><img src="../../../assets/images/icon2.jpg"></p>
            <p><button @click="register()">登录</button></p>
            <p class="fogetpwd"><router-link to='/forgetPwd/1'>忘记密码？</router-link></p>
        </div>
    </div>
</template>

<script>
import { LoginByPhone } from '../../../api/login'
export default {
  data () {
    return {
      phone: '',
      pwd: '',
      phoneRed: false,
      pwdRed: false
    }
  },
  methods: {
    clear (val) {
      switch (val) {
        case 'phone':
          if (this.phoneRed) {
            this.phone = ''
            this.phoneRed = false
          }
          break
        case 'pwd':
          if (this.pwdRed) {
            this.pwd = ''
            this.pwdRed = false
          }
          break
      }
    },
    register () {
      if (this.phone === '') {
        this.phone = '手机号不能为空'
        this.phoneRed = true
      } else {
        if (!(/^1[3|4|5|7|8][0-9]\d{8}$/.test(this.phone))) {
          this.phone = '请填写正确的手机号码'
          this.phoneRed = true
          return
        }
        if (this.pwd === '') {
          this.pwd = '密码不能为空'
          this.pwdRed = true
        } else {
          var options = {
            phone: this.phone,
            password: this.pwd
          }
          // 登录发送数据判断用户与密码是否正确
          LoginByPhone(options).then((res) => {
            if (!res.data['error_code']) {
              localStorage.setItem('user', this.phone)
              this.$router.push('/shopping')
            } else {
              this.phone = '手机号码或密码不正确'
              this.phoneRed = true
              this.pwd = ''
            }
          }, () => {
            console.log('请求失败')
          })
        }
      }
    }
  }
}
</script>

<style scoped="scoped">
    .box {
        width: 100%;
    }
    .red {
        color: red;
    }
    .box p {
        width: 90%;
        margin: 10px 5% 0;
        position: relative;
        line-height: 40px;
        height: 40px;
        padding: 5px 0;
        text-align: center;
    }
    
    input {
        border: none;
        border-bottom: 1px solid #dddddd;
        width: 100%;
        height: 100%;
        line-height: 100%;
        text-align: center;
    }
    
    .box p img {
        position: absolute;
        left: 0;
        top: 10px;
        height: 30px;
    }
    
    
    button {
        width: 90%;
        margin: 0 5%;
        background: #ca3232;
        color: white;
        border: none;
        border-radius: 4px;
        line-height: 40px;
    }
    
    #zhuce input {
        text-align: left;
        box-sizing: border-box;
        padding-left: 15%;
    }
    
    .fogetpwd {
        font-size: 12px;
    }
</style>