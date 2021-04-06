<template>
  <div class="login-container">
    <div class="login-box">
      <div class="avatar-box">
        <img src="../assets/images/logo.png" alt="" />
      </div>
      <el-form
        ref="loginFormRef"
        class="login-form"
        :model="loginForm"
        :rules="loginFormRules"
      >
        <el-form-item prop="username">
          <el-input
            v-model="loginForm.username"
            prefix-icon="el-icon-user"
          ></el-input>
        </el-form-item>
        <el-form-item prop="password">
          <el-input
            v-model="loginForm.password"
            type="password"
            prefix-icon="el-icon-lock"
          ></el-input>
        </el-form-item>
        <el-form-item class="btns">
          <el-button type="primary" @click="login">登录</el-button>
          <el-button type="info" @click="resetLoginForm">重置</el-button>
        </el-form-item>
      </el-form>
    </div>
  </div>
</template>

<script>
export default {
  data() {
    return {
      loginForm: {
        username: 'admin',
        password: '123456'
      },
      loginFormRules: {
        username: [
          { required: true, message: '请输入登录名称', trigger: 'blur' },
          { min: 3, max: 10, message: '长度在 3 到 10 个字符', trigger: 'blur' }
        ],
        password: [
          { required: true, message: '请输入登录密码', trigger: 'blur' },
          { min: 6, max: 15, message: '长度在 6 到 10 个字符', trigger: 'blur' }
        ]
      }
    }
  },
  methods: {
    resetLoginForm() {
      this.$refs.loginFormRef.resetFields()
    },
    login() {
      this.$refs.loginFormRef.validate(async (valid) => {
        if (!valid) return
        const { data: res } = await this.$http.post('login', this.loginForm)
        if (res.meta.status !== 200) return this.$message.error('登录失败！')
        this.$message.success('登录成功')
        window.sessionStorage.setItem('token', res.data.token)
        this.$router.push('/home')
      })
    }
  }
}
</script>

<style lang="scss" scoped>
.login-container {
  height: 100%;
  background-color: #2b4b6b;
  .login-box {
    position: absolute;
    left: 50%;
    top: 50%;
    width: 450px;
    height: 300px;
    background-color: #fff;
    border-radius: 3px;
    transform: translate(-50%, -50%);
    .avatar-box {
      position: absolute;
      left: 50%;
      height: 130px;
      width: 130px;
      padding: 10px;
      border: 1px solid #eee;
      border-radius: 50%;
      box-shadow: 0 0 10px #ddd;
      transform: translate(-50%, -50%);
      background-color: #fff;
      img {
        width: 100%;
        height: 100%;
        background-color: #eee;
        border-radius: 50%;
      }
    }
    .login-form {
      box-sizing: border-box;
      position: absolute;
      bottom: 0;
      width: 100%;
      padding: 0 20px;
      .btns {
        display: flex;
        justify-content: flex-end;
      }
    }
  }
}
</style>