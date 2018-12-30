<template>
  <div class="form_wrap">
    <el-form label-position="top" label-width="80px" :model="formData" class="form">
      <h2 class="title_login">用户登录</h2>
      <el-form-item label="用户名">
        <el-input v-model="formData.username"></el-input>
      </el-form-item>
      <el-form-item label="密码">
        <el-input v-model="formData.password" type="password"></el-input>
      </el-form-item>
      <el-button type="primary" class="btn" @click='login'>登录</el-button>
    </el-form>
  </div>
</template>
<script>
  export default {
    data() {
      return {
        formData: {
          username: '',
          password: ''
        }
      }
    },
    methods: {
      async login() {
        const res = await this.$http.post('login', this.formData)
        console.log(res)
        const { data: { data:{token}, meta: { msg, status } } } = res
        if (status === 200) {
          localStorage.setItem('token',token)

          this.$router.push({
            name: 'home'
          })
        } else {
          this.$message.error(msg)
        }

      }
    },

  }
</script>
<style>
  .form_wrap {
    height: 100%;
    display: flex;
    justify-content: center;
    align-items: center;
    background-color: #324152;
  }

  .title_login {
    text-align: center;
  }

  .form {
    width: 400px;
    border-radius: 8px;
    background-color: #fff;
    padding: 30px
  }

  .btn {
    width: 100%
  }
</style>