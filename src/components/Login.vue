<template>
<div class="login-container">
    <div class="loginLeft">
        <img src="../assets/imgs/login_img.png" >

    </div>
    <div class="loginRight">
        <div class="avatarBox">
            <img src="../assets/imgs/login_logo.png">
        </div>
        <div class="title">
            电商后台管理系统
        </div>
        <el-form ref="loginFormRef" class="loginForm" :model="loginForm" :rules="loginFormRules"> 
            <el-form-item prop="username">
                <el-input placeholder="请输入用户名" prefix-icon="iconfont icon-user" v-model="loginForm.username"></el-input>
            </el-form-item>
            <el-form-item prop="password">
               <el-input placeholder="请输入密码" show-password prefix-icon="iconfont icon-lock_fill" v-model="loginForm.password"></el-input>
            </el-form-item>
            <el-form-item>
                <el-button type="primary" class="loginBtn" @click="login">登陆</el-button>
            </el-form-item>
        </el-form>    
    </div>
    
</div>
    
</template>

<script>
export default {
    data(){
        return{
            loginForm:{
                username:'',
                password:''
            },
            loginFormRules:{
                username:[
                    { required: true, message: '请输入登录账户', trigger: 'blur' },
                    { min: 3, max: 15, message: '长度在 3 到 15 个字符', trigger: 'blur' }
                ],
                password:[
                    { required: true, message: '请输入登录密码', trigger: 'blur' },
                    { min: 6, max: 15, message: '长度在 6 到 15 个字符', trigger: 'blur' }
                ]
            }
        }
    },
    methods:{
        login(){
            this.$refs.loginFormRef.validate(async valid=>{
                if(!valid){
                    return
                }
                console.log('发送登陆请求')
                const {data:res} =  await this.$http.post('login',this.loginForm)
                if(res.meta.status !== 200){
                    return  this.$message.error(res.meta.msg);
                }
                this.$message({message: res.meta.msg,type: 'success'});
                console.log('res:',res)
                sessionStorage.setItem('userInfo',JSON.stringify(res.data))
                this.$router.push('/home')
            })
        }
    }
};
</script>

<style lang="less" scoped>
.login-container {
  background-color: #93defe;
  height: 100%;
  display: flex;
  justify-content: center;
  align-items: center;
  .loginRight {
    margin-left: 60px;
    width: 400px;
    height: 350px;
    background-color: #ffffff;
    padding: 50px;
    border-radius: 6px;
    box-sizing: border-box;
    position: relative;
    .avatarBox {
      position: absolute;
      width: 120px;
      height: 120px;
      border: 5px solid #93defe;
      border-radius: 100px;
      text-align: center;
      line-height: 110px;
      top: -60px;
      right: 140px;
      background-color: #ffffff;
    }
    .title {
      position: absolute;
      top: 100px;
      left: 50%;
      transform: translate(-50%);
      font-size: 18px;
      color: #444444;
    }
    .loginForm{
        position: absolute;
        bottom: 0;
        left: 50%;
        transform: translate(-50%);
        padding: 0 20px;
        width: 80%;
        box-sizing: border-box;
        .loginBtn{
            width: 100%;
        }
    }
  }
}
</style>