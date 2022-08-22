<template>
  <div id="app">
    <el-card class="login-card">
      <!--!三个先决条件 1 model rules 2 prop 3 model-->
      <el-form :model="loginForm" :rules="rules" ref="login">
        <el-form-item prop="iphone">
          <el-input placeholder="请输入手机号" v-model="loginForm.iphone"></el-input>
        </el-form-item>
        <el-form-item prop="password">
          <el-input placeholder="请输入密码" v-model="loginForm.password"></el-input>
        </el-form-item>
        <el-form-item >
          <el-button type="primary" style="width:100%" @click="loginvalidate">登录</el-button>
        </el-form-item>
      </el-form>
    </el-card>
  </div>
</template>
<script>
export default {
  name: 'App',
  data() {
    //* 自定义校验规则 validator函数 rule value callback 三个参数
    const logins = (rule,value,callback) => {
      value.charAt(2) === '9' ? callback() : callback(new Error('第三位数字为9'))
    }
    return {
      loginForm: {
        iphone: '',
        password: ''
      },
      //! 校验规则格式 对象->数组对象格式
      rules: {
        //! 设置校验规则 触发前 触发后
        iphone: [
          {required: true, message: "请输入手机号", trigger:"blur"},
          {pattern: /^1[3-9]\d{9}$/,message: '第二位是在3-9之间的数字',trigger: 'blur'},
          {validator: logins, trigger: 'blur'}
        ],
        password: [
          {min:6, max:16,message: '请输入正确密码',trigger: 'blur'}
        ]
      }    
    }
  },
  methods: {
    //! 如果没有点击校验 直接点击登录 则成功 需要 利用form表单内置API validate()进行设置 默认接收一个callback 不传参则返回promise对象
    loginvalidate() {
      this.$refs.login.validate( isOk => {
        if(isOk) {
          console.log('登录成功')
        }else {
          console.log('登录失败')
        }
      })
    }
  }
}
</script>

<style>
#app {
  width: 100%;
  height: 100vh;
  background-color: #666;
  display: flex;
  justify-content: center;
  align-items: center;
}
.login-card {
  width: 440px;
  height: 300px;
}
/* .login-button {
  align-items: center;
} */
</style>
