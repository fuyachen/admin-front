<template>
  <div class="login-container">
    <div class="modal">
      <el-form :model="user" :rules="rules" status-icon ref="userForm">
        <div class="title">后台管理系统</div>
        <el-form-item prop="userName">
          <el-input
            type="text"
            :prefix-icon="User"
            v-model="user.userName"
            placeholder="管理员账号：admin"
          />
        </el-form-item>
        <el-form-item prop="userPwd">
          <el-input
            type="password"
            :prefix-icon="View"
            v-model="user.userPwd"
            placeholder="密码：123"
          />
        </el-form-item>
        <el-form-item>
          <el-button class="btn-login" type="primary" @click="login()"
            >提交</el-button
          >
        </el-form-item>
      </el-form>
    </div>
  </div>
</template>

<script setup>
import { User, View } from "@element-plus/icons-vue"
import { reactive, ref } from "vue"
import api from "../api"
import { useStore } from "vuex"
import { useRouter } from "vue-router"
import { ElMessage } from "element-plus"

const store = useStore()
const router = useRouter()

const user = reactive({
  userName: "",
  userPwd: "",
})

const rules = {
  userName: [{ required: true, message: "请输入用户名", trigger: "blur" }],
  userPwd: [{ required: true, message: "请输入密码", trigger: "blur" }],
}

const userForm = ref()

function login() {
  userForm.value.validate((isValid) => {
    if (isValid) {
      api
        .login({
          userName: user.userName,
          userPwd: user.userPwd,
        })
        .then((res) => {
          store.commit("saveUserInfo", res)
          router.push("./welcome")
        })
        .catch((err) => {
          ElMessage.error("登陆失败：", err)
        })
    } else {
      return false
    }
  })
}
</script>

<style lang="scss">
.login-container {
  display: flex;
  justify-content: center;
  align-items: center;
  background-color: #f9fcff;
  width: 100vw;
  height: 100vh;
  .modal {
    position: relative;
    width: 400px;
    padding: 50px;
    background-color: #fff;
    border-radius: 10px;
    box-shadow: 0px 0px 10px 3px #c7c9cb4d;
    .title {
      font-size: 25px;
      line-height: 1.5;
      color: rgba(17, 24, 39, 0.77);
      text-align: center;
      margin-bottom: 30px;
    }
    .btn-login {
      width: 100%;
    }
  }
}
</style>
