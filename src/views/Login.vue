<template>
  <div class="login">
    <section class="form_container">
      <div class="manage_tip">
        <span class="title">陆向谦创新创业管理系统</span>

        <el-form
          :model="loginUser"
          status-icon
          :rules="formRules"
          ref="loginForm"
          label-width="80px"
          class="loginForm"
        >
          <el-form-item label="邮箱" prop="email">
            <el-input v-model="loginUser.email" aplaceholder="请输入email"></el-input>
          </el-form-item>
          <el-form-item label="密码" prop="password">
            <el-input type="password" v-model="loginUser.password" placeholder="请输入密码"></el-input>
          </el-form-item>

          <el-form-item>
            <el-button type="primary" class="submit_btn" @click="submitForm('loginForm')">登录</el-button>
          </el-form-item>
          <div class="tip_register">
            <p>
              还没有账号？现在
              <router-link to="/register">注册</router-link>
            </p>
          </div>
        </el-form>
      </div>
    </section>
  </div>
</template>

<script>
import jwt_decode from "jwt-decode";

export default {
  name: "register",
  components: {},
  data() {
    return {
      loginUser: {
        email: "",
        password: ""
      },
      formRules: {
        email: [
          {
            type: "email",
            required: true,
            message: "邮箱格式不正确",
            trigger: "blur"
          }
        ],
        password: [
          {
            required: true,
            message: "密码不能为空",
            trigger: "blur"
          },
          {
            min: 6,
            max: 30,
            message: "密码长度在6-30之间",
            trigger: "blur"
          }
        ]
      }
    };
  },
  methods: {
    submitForm(formName) {
      this.$refs[formName].validate(valid => {
        if (valid) {
            var param = {
                username: 'aaa',
                password: '123456'
            }
          this.$axios
            .post("/sys_back/log/login", param)
            .then(response => {
              console.log(response);
              // token
              // const { token } = res.data;
              const token = "32fsfsd88dd85f8ser5gdv7g";
          localStorage.setItem("token", token);
          // 解析token
          const decoded = jwt_decode(token);
          // console.log(decoded)

          // token存储在vuex中 decoded是空的话，代表没授权，有内容的话 有授权
          this.$store.dispatch("setAuthenticated", !this.isEmpty(decoded));
          this.$store.dispatch("setUser", decoded);
          this.$router.push("/index");
            });
          this.$router.push("/index");
        } else {
          console.log("error submit!!");
          return false;
        }
      });
    },
    isEmpty(value) {
      return (
        value === undefind ||
        value === null ||
        (typeof value === "object" && Object.keys(value).length === 0) ||
        (typeof value === "string" && value.trim().length === 0)
      );
    }
  }
};
</script>
<style scoped>
.login {
  position: relative;
  width: 100%;
  height: 100%;
}
.form_container {
  width: 370px;
  height: 210px;
  position: absolute;
  top: 20%;
  left: 34%;
  padding: 25px;
  border-radius: 5px;
  text-align: center;
}
.form_container .manage_tip .title {
  font-family: "Microsoft YaHei";
  font-weight: bold;
  font-size: 26px;
  color: #fff;
}
.loginForm {
  margin-top: 20px;
  background-color: #fff;
  padding: 20px 40px 20px 20px;
  border-radius: 5px;
  box-shadow: 0px 5px 10px #cccc;
}

.submit_btn {
  width: 100%;
}
.tip_register {
  text-align: right;
  font-size: 12px;
  color: #333;
}
.tip_register p a {
  color: #409eff;
}
</style>