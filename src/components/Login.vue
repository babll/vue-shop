<template>
  <div class="login_container">
    <div class="login_box">
      <!-- 头像区域 -->
      <div class="avatar_box">
        <img src="../assets/logo.png" alt="">
      </div>
      <!-- 登录表单区域 -->
      <el-form ref="loginFromRef" :model="loginFrom" :rules="loginFromRules" label-width="0px" class="login_from">
        <el-form-item prop="username">
          <el-input v-model="loginFrom.username" prefix-icon="el-icon-s-custom">
          </el-input>
        </el-form-item>
        <el-form-item prop="password">
          <el-input v-model="loginFrom.password" prefix-icon="el-icon-lock" type="password">
          </el-input>
        </el-form-item> 
        <el-form-item class="btns">
          <el-button type="primary" @click="login">登录</el-button>
          <el-button type="info" @click="resetLoginFrom">重置</el-button>
        </el-form-item>
      </el-form>
    </div>
  </div>
</template>
<script>
export default {
  data(){
    return{
      loginFrom:{
        username:'',
        password:''
      },
      loginFromRules:{
        username:[
          {required:true,message:'请输入用户名',trigger:'blur'},
          {min:3,max:10,message:'用户名长度3到10位',trigger:'blur'}
          ],
        password:[
          {required:true,message:'请输入密码',trigger:'blur'},
          {min:6,max:15,message:'密码长度6到15位',trigger:'blur'}
        ]
      }
    }
  },
  methods:{
    resetLoginFrom(){
      this.$refs.loginFromRef.resetFields()
    },
    login(){
      this.$refs.loginFromRef.validate((valid)=>{
        if(!valid) return;
        this.$http.post('login',this.loginFrom).then(res=>{
          console.log(res.data)
          if(res.data.meta.status !== 200){
            this.$message.error('登录失败')
            return
          }
          this.$message.success('登录成功')
          window.sessionStorage.setItem('token',res.data.data.token)
          this.$router.push("/home")
        }).catch(err=>{
          console.log(err)
        })
      })
    }
  }
}
</script>
<style lang="less" scoped>
.login_container{
  background-color: #2b4b6b;
  height: 100%;
}
.login_box{
  width:450px;
  height: 300px;
  background-color: #fff;
  border-radius: 3px;
  position: absolute;
  top:50%;
  left:50%;
  transform: translate(-50%,-50%);
  .avatar_box{
    height: 130px;
    width: 130px;
    border: 1px solid #eee;
    border-radius: 50%;
    padding: 10px;
    box-shadow: 0 0 10px #ddd;
    position: absolute;
    left: 50%;
    transform: translate(-50%,-50%);
    background-color: #fff;
    img{
      width: 100%;
      height: 100%;
      border-radius: 50%;
      background-color: #eee;
    }
  }
}
.login_from{
  position: absolute;
  bottom: 0;
  width: 100%;
  padding: 0 20px;
  box-sizing: border-box;
}
.btns{
  display: flex;
  justify-content:space-between
}
</style>
