<template>
<div class='container'>
    <el-card class="my-card">
     <img src="../../assets/images/logo_index.png" alt="">
    <el-form :model="loginForm"  :rules="loginRules" ref="loginForm" status-icon>
      <el-form-item prop="mobile">
        <el-input v-model="loginForm.mobile" placeholder="请输入正确电话号码"></el-input>
      </el-form-item>
      <el-form-item prop="code">
        <el-input v-model="loginForm.code" placeholder="请输入正确验证码" style="width:235px; margin-right:12px"></el-input>
        <el-button>发送验证码</el-button>
      </el-form-item>
      <el-form-item>
         <el-checkbox :value="true">我已阅读并同意用户协议和隐私条款</el-checkbox>
      </el-form-item>
      <el-form-item>
        <el-button type="primary" style="width:100%" @click="login()">登录</el-button>
      </el-form-item>
    </el-form>
    </el-card>
</div>
</template>

<script>
export default {
  data () {
    const checkMobile = (rule, value, callback) => {
      if (!/^1[3-9]\d{9}$/.test(value)) return callback(new Error('手机格式不对'))
      callback()
    }
    return {
      loginForm: {
        mobile: '',
        code: ''
      },
      loginRules: {
        mobile: [
          { required: true, message: '请输入手机号', trigger: 'blur' },
          { validator: checkMobile, trigger: 'blur' }
        ],
        code: [
          { required: true, message: '请输入验证码', trigger: 'blur' },
          { len: 6, message: '请输入6位验证码', trigger: 'blur' }
        ]
      }
    }
  },
  methods: {
    login () {
      this.$refs.loginForm.validate((valid) => {
        if (valid) {
          this.$http.post('http://ttapi.research.itcast.cn/mp/v1_0/authorizations', this.loginForm).then(res => {
            this.$router.push('/')
          })
            .catch(() => {
              this.$message.error('手机号或验证码错误')
            })
        }
      })
    }
  }
}
</script>

<style scoped lang='less'>
.container {
    width: 100%;
    height: 100%;
    position: absolute;
    background: url(../../assets/images/login_bg.jpg) no-repeat  center / cover;
    .my-card {
       width: 400px;
       height: 400px;
       position: absolute;
       left: 50%;
       top: 50%;
       transform: translate(-50%,-60%);
       img {
         width: 200px;
         display: block;
         margin: 0 auto 30px;
       }
    }
}

</style>
