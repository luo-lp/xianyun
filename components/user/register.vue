<template>
  <el-form :model="form" ref="form" :rules="rules" class="form">
    <el-form-item class="form-item">
      <el-input placeholder="用户名手机" v-model="form.username"></el-input>
    </el-form-item>

    <el-form-item class="form-item">
      <el-input placeholder="验证码" v-model="form.nickname">
        <template slot="append">
          <el-button @click="handleSendCaptcha">发送验证码</el-button>
        </template>
      </el-input>
    </el-form-item>

    <el-form-item class="form-item">
      <el-input placeholder="你的名字" v-model="form.captcha"></el-input>
    </el-form-item>

    <el-form-item class="form-item">
      <el-input placeholder="密码" type="password" v-model="form.password"></el-input>
    </el-form-item>

    <el-form-item class="form-item">
      <el-input placeholder="确认密码" type="password" v-model="form.psw"></el-input>
    </el-form-item>

    <el-button class="submit" type="primary" @click="handleRegSubmit">注册</el-button>
  </el-form>
</template>

<script>
export default {
  data() {
    return {
      // 表单数据
      form: {
        username: "15521192978",
        nickname: "0011",
        captcha: "000000",
        password: "123456789",
        psw: ""
      },
      code: "",
      // 表单规则
      rules: {}
    };
  },
  methods: {
    // 发送验证码
    handleSendCaptcha() {
      this.$axios({
        url: "captchas",
        method: "POST",
        data: {
          tel: "15521192978"
        }
      }).then(res => {
        console.log(res);
      });
    },

    // 注册
    handleRegSubmit() {
      // console.log(this);
      const { psw, ...data } = this.form;
      this.$store.dispatch("user/reg", data).then(res => {
        // 成功提示
        console.log(res);
        this.$message({
          message: "登录成功，正在跳转",
          type: "success"
        });
        // 跳转到首页
        setTimeout(() => {
          this.$router.replace("/")
        }, 1000);
      });
    }
  }
};
</script>

<style scoped lang="less">
.form {
  padding: 25px;
}

.form-item {
  margin-bottom: 20px;
}

.form-text {
  font-size: 12px;
  color: #409eff;
  text-align: right;
  line-height: 1;
}

.submit {
  width: 100%;
  margin-top: 10px;
}
</style>