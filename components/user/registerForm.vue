<template>
  <el-form :model="form" ref="form" :rules="rules" class="form">
    <el-form-item class="form-item" prop="username">
      <el-input placeholder="用户名手机" v-model="form.username"></el-input>
    </el-form-item>
    <el-form-item class="form-item" prop="captcha">
      <el-input placeholder="验证码" v-model="form.captcha">
        <template slot="append">
          <el-button @click="handleSend">发送验证码</el-button>
        </template>
      </el-input>
    </el-form-item>
    <el-form-item class="form-item" prop="nickname">
      <el-input placeholder="你的名字" v-model="form.nickname"></el-input>
    </el-form-item>

    <el-form-item class="form-item" prop="password">
      <el-input placeholder="密码" type="password" v-model="form.password"></el-input>
    </el-form-item>
    <el-form-item class="form-item" prop="checkPassword">
      <el-input type="password" placeholder="确认密码" v-model="form.checkPassword"></el-input>
    </el-form-item>

    <el-button class="submit" type="primary" @click="handleRegister">注册</el-button>
  </el-form>
</template>

<script>
export default {
  data() {
    return {
      form: {
        username: "",
        nickname: "",
        captcha: "",
        password: "",
        checkPassword:'',
      },
      rules: {
        username: [{ required: true, message: "请输入正确手机号", trigger: "blur" }],
        captcha:[{ required: true, message: "请输入验证码", trigger: "blur" }],
        nickname:[{ required: true, message: "请输入名称", trigger: "blur" }],

        password: [{ required: true, message: "请输入密码", trigger: "blur" }],
        checkPassword:[{ required: true, message: "请再次输入密码", trigger: "blur" }]
      }
    };
  },

  methods:{
      handleSend() {
           if(!this.form.username.trim()){
                 this.$confirm('手机号码不能为空','提示',{
                     confirmButtonText: '确定',
                     showCancelButton:false,
                     type: 'warning',
                 })
                 return 
           }
           if(this.form.username.length !== 11){
                 this.$confirm('手机必须为11位数','提示',{
                     confirmButtonText: '确定',
                     showCancelButton:false,
                     type: 'warning',
                 })
                 return 
           }
           this.$axios({
                url:'/captchas',
                method:'POST',
                data:{
                    tel: this.form.username
                }
           }).then(res => {
                this.$confirm(res.data.code,'验证码',{
                     confirmButtonText: '确定',
                     showCancelButton:false,
                     type: 'success',
                 })
           })
      },
      handleRegister(){
          this.$refs.form.validate(valid => {
              if(valid) {
                   const {checkPassword,...props} = this.form
                   this.$axios({
                       url:'/accounts/register',
                       method:'POST',
                       data:props
                   }).then(res => {
                        this.$store.dispatch("user/setUserInfoAction", res.data);
                        this.$router.push('/')
                   })
              } 
         
          })
      }
  }

};
</script>
<style lang="less" scoped>
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
}
</style>
