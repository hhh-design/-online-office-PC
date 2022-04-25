<template>
  <div class="login">
    <div class="login-box">
      <div class="logo">
        <img src="../static/images/loading2.gif" />
      </div>
      <div class="body">
        <p class="tips">移动办公后台管理系统</p>
        <el-form ref="form" :model="form" :rules="rules" label-position="top">
          <el-form-item label="" prop="username">
            <el-input v-model="form.username" placeholder="用户名"></el-input>
          </el-form-item>
          <el-form-item label="" prop="password">
            <el-input type="password" v-model="form.password" placeholder="密码"></el-input>
          </el-form-item>
          <el-form-item>
            <el-button type="primary" @click="startLogin" :loading="loading" style="width: 100%">
              {{ loading ? '登录中...' : '登录' }}
            </el-button>
          </el-form-item>
        </el-form>
        <p class="foottip"><span>用户名：wjt1234</span> <span>密码：wjt1234</span></p>
      </div>
    </div>
  </div>
</template>
<script>
import api from '@/config/api'

export default {
  data() {
    return {
      root: '',
      form: {
        username: 'wjt1234',
        password: 'wjt1234'
      },
      rules: {
        username: [{ required: true, message: '请输入用户名', trigger: 'blur' }],
        password: [
          { required: true, message: '请输入密码', trigger: 'blur' },
          { min: 6, message: '密码不得低于6个字符', trigger: 'blur' }
        ]
      },
      loading: false
    }
  },
  components: {},
  methods: {
    startLogin() {
      this.$refs['form'].validate(valid => {
        if (!valid) {
          return false
        }
        this.loading = true
        let root = this.root
        this.axios
          .post(root + 'auth/login', {
            username: this.form.username,
            password: this.form.password
          })
          .then(res => {
            let call = res.data
            console.log(call)
            this.loading = false
            if (res.data.errno === 0) {
              console.log(res.data.data)
              localStorage.setItem('token', res.data.data.token)
              localStorage.setItem('userInfo', JSON.stringify(res.data.data.userInfo))
              console.log(JSON.stringify(res.data.data.token))
              console.log(JSON.stringify(res.data.data.userInfo))
              this.$router.push({ name: 'welcome' })
              let sUserAgent = navigator.userAgent
              // todo 手机端
              let mobileAgents = ['Android', 'iPhone', 'Symbian', 'WindowsPhone', 'iPod', 'BlackBerry', 'Windows CE']
              let goUrl = 0
              for (var i = 0; i < mobileAgents.length; i++) {
                if (sUserAgent.indexOf(mobileAgents[i]) > -1) {
                  goUrl = 1
                  break
                }
              }
              console.log(goUrl)
              if (goUrl == 1) {
                this.$router.push({ name: 'wap' })
              }
            } else {
              this.$message({
                type: 'error',
                message: call.errmsg
              })
              return false
            }
          })
          .catch(err => {
            this.loading = false
          })
      })
    }
  },
  mounted() {
    this.root = api.rootUrl
  }
}
</script>
<style>
.login {
  height: 100vh;
  width: 100vw;
  /* 背景图片是自己设定的 */
  background: url('http://r9tekfe7i.hd-bkt.clouddn.com/1649314972108.jpg');
  background-size: cover;
  background-repeat: no-repeat;
  background-position: center;
  display: flex;
  font-family: Lato, Helvetica, sans-serif;
  justify-content: center;
  align-items: center;
  text-align: center;
  color: #656565;
  position: relative;
}

.login-box {
  width: 320px;
  background: #ffffff7a;
  -webkit-border-radius: 10px;
  -moz-border-radius: 10px;
  border-radius: 10px;
  box-shadow: 2px 2px 12px #ccc;
}

.login-box .logo {
  height: 100px;
  padding-top: 30px;
  /*background: #324157;*/
  display: flex;
  align-items: center;
  justify-content: center;
}

.login-box .logo img {
  width: 80px;
  height: 80px;
}

.login-box .body {
  padding: 10px 30px 30px 30px;
}

.login-box .body .tips {
  font-size: 14px;
  height: 40px;
  line-height: 40px;
  text-align: center;
  margin-bottom: 30px;
}
.login-box .body .foottip {
  font-size: 14px;
  display: flex;
  justify-content: space-around;
}

.login-box .body .author {
  display: block;
  font-size: 14px;
  height: 40px;
  line-height: 40px;
  text-align: center;
  color: #656565;
  margin-bottom: 10px;
  text-decoration: none;
}

.login-box .body .author a {
  text-decoration: none;
}
</style>
