<template>

  <div class="login_container">
    <div class="login_box">
      <div class="avatar_box">
        <!-- 头像区域-->
        <img alt="#" src="../assets/images/logo.png">
      </div>

      <!-- 登录表单区域-->
      <el-form ref="loginFormRef" :model="loginForm" :rules="loginFormRules" class="login_form" label-width="0px">
        <!-- 用户名-->
        <el-form-item prop="username">
          <el-input v-model="loginForm.username" prefix-icon="iconfont icon-user"></el-input>
        </el-form-item>
        <!-- 密码-->
        <el-form-item prop="password">
          <el-input v-model="loginForm.password" prefix-icon="iconfont icon-3702mima" type="password"></el-input>
        </el-form-item>

        <!-- 登录按钮区域-->
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
  name: 'Login',
  data () {
    return {
      // 这是登录表单的数据绑定对象
      loginForm: {
        // ESLint：字符串必须使用单引号。(quotes)
        username: 'admin',
        password: '123456'
      },
      // 这是表单验证规则对象
      loginFormRules: {
        // 验证用户名是否合法
        username: [
          {
            required: true,
            message: '请输入登录名称',
            trigger: 'blur'
          },
          {
            min: 3,
            max: 10,
            message: '用户名长度在 3 到 10 个字符',
            trigger: 'blur'
          }
        ],
        // 验证密码是否合法
        password: [
          {
            required: true,
            message: '请输入密码',
            trigger: 'blur'
          },
          {
            min: 6,
            max: 16,
            message: '密码长度在 6 到 16 个字符',
            trigger: 'blur'
          }
        ]
      }
    }
  },
  methods: {
    /**
     * 点击重置按钮, 重置登录表单
     */
    resetLoginForm: function () {
      // console.log(this);
      this.$refs.loginFormRef.resetFields()
    },
    /**
     * 实现登录前表单数据的预验证
     */
    login: function () {
      this.$refs.loginFormRef.validate(async (valid) => {
        // console.log(valid)
        if (!valid) {
          return
        }
        const { data: res } = await this.$http.post('login', this.loginForm)
        // const result = await this.$http.post('login', this.loginForm)
        // console.log(result.data.meta.status)
        // console.log('-----------------------')
        // console.log(res)
        if (res.meta.status !== 200) {
          return this.$message.error('登录失败')
        } else {
          this.$message.success('登录成功')
          // 1.将登录成功之后的token, 保存到客户端的sessionStorage中
          //  1.1.项目中出了登录之外的其他API接口, 必须在登录之后才能访问
          //  1.2.token只应在当前网站打开期间生效, 所以将token保存到sessionStorage中
          window.sessionStorage.setItem('token', res.data.token)
          // 2.通过编程式导航跳转到后台主页, 路由地址是 /home
          this.$router.push('/home')
        }
      })
    }
  }

}
</script>

<style lang="less" scoped>
.login_container {
  width: 100%;
  height: 100%;
  background-image: url('../assets/images/login.jpg');
  background-size: 100% 100%;
}

.login_box {
  width: 450px;
  height: 300px;
  background-color: #fff;
  border-radius: 5px;
  position: absolute;
  left: 50%;
  top: 47%;
  transform: translate(-50%, -50%);
  box-shadow: 5px 5px 60px 0px #e9eaeb;

  .avatar_box {
    height: 130px;
    width: 130px;
    border: 1px solid #eee;
    border-radius: 50%;
    padding: 10px;
    box-shadow: 0 0 10px #ddd;
    position: absolute;
    left: 50%;
    transform: translate(-50%, -50%);
    background-color: #fff;

    img {
      height: 100%;
      width: 100%;
      border-radius: 50%;
      background-color: #eee;
    }
  }
}

.btns {
  display: flex;
  justify-content: center;
}

.login_form {
  position: absolute;
  width: 100%;
  bottom: 0;
  padding: 0 20px;
  box-sizing: border-box;
}
/deep/.el-input {
  width: 350px;
  margin-left: 32px;
}
</style>
