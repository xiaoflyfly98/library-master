<template>
 <div class="login-page">
  <div style="height: 100vh; overflow: hidden; position: relative; opacity: 0.97">
    <el-card class="cover" v-if="loginAdmin.id">
      <slide-verify :l="42"
                    :r="10"
                    :w="310"
                    :h="155"
                    :accuracy="5"
                    :imgs="images"
                    slider-text="向右滑动"
                    @success="onSuccess"
                    @fail="onFail"
                    @refresh="onRefresh"
      ></slide-verify>
    </el-card>

      <div style="width: 500px; height: 400px; background-color: white; border-radius: 10px;
        margin: 150px auto; padding:50px">
        <!-- <div style="margin: 30px; text-align: center; font-size: 30px; font-weight: bold; color: dodgerblue">登 录</div> -->
        <div class="login__container">
          <img class="login__img" src="../../assets/logo.png">
          <div class="login__content" style="margin: 10px; text-align: center;  font-weight: bold; color: dodgerblue">科城图书管理系统,欢迎您！</div>
        </div>
        <el-form :model="admin" :rules="rules" ref="loginForm">
          <el-form-item prop="username">
            <el-input placeholder="请输入账号" prefix-icon="el-icon-user" size="medium" v-model="admin.username"></el-input>
          </el-form-item>
          <el-form-item prop="password">
            <el-input placeholder="请输入密码" show-password prefix-icon="el-icon-lock" size="medium" v-model="admin.password"></el-input>
          </el-form-item>
          <el-form-item>
            <el-button style="width: 100%" size="medium" type="primary" @click="login">登录</el-button>
          </el-form-item>
        </el-form>
      </div>
    </div>
  </div>
</template>

<script>
import request from "@/utils/request";
import Cookies from 'js-cookie'

export default {
  name: 'LOGIN',
  data() {
    return {
      loginAdmin: {},
      admin: {},
      images: [
        require('../../assets/cdn/test1.jpg'),
        require('../../assets/cdn/test2.jpg'),
        require('../../assets/cdn/test3.jpg'),
        require('../../assets/logo.png')
      ],
      rules: {
        username: [
          { required: true, message: '请输入用户名', trigger: 'blur'},
          { min: 3, max: 10, message: '长度在3-10个字符', trigger: 'blur'}
        ],
        password: [
          { required: true, message: '请输入密码', trigger: 'blur'},
          { min: 3, max: 10, message: '长度在3-10个字符', trigger: 'blur'}
        ]
      }
    }
  },
  methods: {
    login() {
      this.$refs['loginForm'].validate((valid) => {
        if (valid) {
          request.post('/admin/login', this.admin).then(res => {
            if (res.code === '200') {
              this.loginAdmin = res.data  // 滑块组件就出现了
            } else {
              this.$notify.error(res.msg)
            }
          })
        }
      })
    },
    onSuccess() { // 滑块验证通过之后触发的
      Cookies.set('admin', JSON.stringify(this.loginAdmin))
      this.$notify.success("登录成功")
      this.$router.push('/')
    },
    onFail() {
      console.log('onFail')
    },
    onRefresh() {
      console.log('refresh')
    }
  }
}
</script>

<style>
.cover {
  width: fit-content;
  background-color: white;
  /* background-image: url(../../assets/background.png); */
  position: absolute;
  top:50%;
  left:50%;
  transform: translate(-50%, -50%);
  z-index: 1000;
}
.login-page {
  background-image: url(../../assets/background.png);
  background-size: cover;
  background-position: center center;
  background-repeat: no-repeat;
  /* opacity: 0.8; */
}
.login__container {
  display: flex;
  flex-direction: column;
  align-items: center;
  justify-content: center;
}
.login__img{
  width: 100px; 
  height:100px; 
}
</style>