<template>
  <div id="login">
    <el-form :model="ruleForm" status-icon :rules="rules" ref="ruleForm" label-width="100px" class="login-container">
      <h2>用户登录</h2>
      <el-form-item label="用户名" prop="username">
        <el-input  v-model="ruleForm.username" auto-complete="off"></el-input>
      </el-form-item>
      <el-form-item label="密码" prop="password">
        <el-input type="Password" v-model="ruleForm.password" auto-complete="off"></el-input>
      </el-form-item>
      <el-form-item>
        <el-button type="primary" plain @click="submitForm('ruleForm')">提交</el-button>
        <el-button plain @click="resetForm('ruleForm')">重新填写</el-button>
      </el-form-item>
    </el-form>
  </div>
</template>
<script>
  import { mapActions } from 'vuex'

  export default {
    data() {
      return {
        ruleForm: {
          username: '',
          password: ''
        },
        rules: {
          username: [
            { required: true, trigger: 'blur' }
          ],
          password: [
            { required: true, trigger: 'blur' }
          ]
        }
      };
    },
    methods: {
      ...mapActions([
        'loginHandle'
      ]),
      submitForm(formName) {
        this.$refs[formName].validate((valid) => {
          if (valid) {
            this.loginHandle({
              ...this.ruleForm,
              ...this.$config[this.$provider].authorize,
              provider: this.$provider
            }).then(result => {
              this.$router.push({
                name: 'memberMain'
              })
            })
          }
        });
      },
      resetForm(formName) {
        this.$refs[formName].resetFields();
      }
    }
  }
</script>
<style scoped>
  h2 {
    text-align: center;
    color: #42b983;
  }
  #login {
    height:100%;
  }
  .login-container {
    width: 350px;
    padding: 20px;
    background: #fff;
    position: absolute;
    top:50%;
    left:50%;
    margin-top:-200px;
    margin-left:-195px;
    border: 1px solid #eaeaea;
    -webkit-border-radius: 5px;
    border-radius: 5px;
    -moz-border-radius: 5px;
  }
</style>
