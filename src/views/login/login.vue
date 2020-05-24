<template>
    <div id="login">
        <div class="login-wrap">
            <ul class="menu-yab">
                <li :class="{current: item.current}" @click="toggleMenu(item)" v-for="item in menuTab" :key="item.id">{{item.txt}} </li>
            </ul>
            <el-form :model="ruleForm" status-icon :rules="rules" ref="ruleForm" l class="demo-ruleForm">
            <el-form-item  prop="username">
                <label >邮箱</label>
                <el-input type="text" v-model="ruleForm.username" autocomplete="off"></el-input>
            </el-form-item>

            <el-form-item  prop="password">
                <label >密码</label>
                <el-input type="password" v-model.trim ="ruleForm.password" autocomplete="off" maxlength="20" minlength="6"></el-input>
            </el-form-item>
            <el-form-item  prop="passwords">
                <label v-show="model=='register'">重复密码</label>
                <el-input type="password" v-model.trim ="ruleForm.passwords" autocomplete="off" maxlength="20" minlength="6" v-show="model=='register'" ></el-input>
            </el-form-item>

            <el-form-item  prop="code">
                <label >验证码</label>
                <el-row :gutter="20">
                <el-col :span="15">
                     <el-input v-model.trim ="ruleForm.code"  maxlength="20" minlength="6"></el-input>
                </el-col>
                <el-col :span="9">
                    <el-button  type="success" class="block" >获取验证码</el-button>    
                </el-col>
                
                </el-row>
               
            </el-form-item>

            <el-form-item>
                <el-button type="primary" @click="submitForm('ruleForm')" class="bolck marginTop">提交</el-button>
                
            </el-form-item>
            </el-form>
        </div>
    </div>
</template>
<script>
import {stripscript ,validateEmail,validatePass,validateVCode} from '@/utils/validate';
export default {
    name:"login",
    data(){
      //验证验证码
        var checkCode = (rule, value, callback) => {
        if (value === '') {
          return callback(new Error('请输入验证码'));
        }else if (validateVCode(value)) {
            callback(new Error('验证码格式有误'));
          } else {
              callback();
            }
      };
      //验证邮箱
      let validateUsername = (rule, value, callback) => {
        if (value === '') {
          callback(new Error('请输入用户名'));
        } else if(validateEmail(value)){
          callback(new Error('用户名格式有误'));
        } else {
          callback(); //true
        }
      };
      //验证重复密码
      let validatePasswords = (rule, value, callback) => {
        if(this.model == 'login'){ callback()};
        if (value === '') {
          callback(new Error("请再次输入密码"));
        } else if (value != this.ruleForm.password) {
          callback(new Error("两次输入密码不相同"));
        } else {
          callback();
        }
      };
      //验证密码
      let validatePassword = (rule, value, callback) => {
        if (value === '') {
          callback(new Error("请输入密码"));
        } else if (validatePass(value)) {
          callback(new Error("密码格式不正确"));
        } else {
          callback();
        }
      };
      return {
        ruleForm: {
          username: '',
          password: '',
          code: '',
          passwords:''
        },
        //模块值
        model:'login',
        rules: {
          username: [
            { validator:validateUsername, trigger: 'blur' }
          ],
          password: [
            { validator: validatePassword, trigger: 'blur' }
          ],
           passwords: [
            { validator: validatePasswords, trigger: 'blur' }
          ],
          code: [
            { validator: checkCode, trigger: 'blur' }
          ]
        },
        menuTab:[
                {txt:"登录",current:true ,type:'login'},
                {txt:"注册",current:false,type:'register'}
            ]
        }
      },
       
    created(){},
    nounted(){},
    methods:{
        submitForm(formName) {
        this.$refs[formName].validate((valid) => {
          if (valid) {
            alert('submit!');
          } else {
            console.log('error submit!!');
            return false;
          }
        });
      },
     
        toggleMenu(data){
            this.menuTab.forEach(element => {
                element.current = false;              
            });
            data.current=true;
            this.model = data.type;
        }
    },
}
</script>
<style lang="scss" scoped>
.menu-tab{
    text-align: center;
}
li{
        text-align: center;
        display: inline-block;
        
        width: 88px;
        line-height: 36px;
        font-size: 14px;
        border-radius: 2px;
        cursor: pointer;
      
    }
.login-wrap{
    width: 330px;
    margin: auto;
}
#login{
    height: 100vh;
    background-image: linear-gradient(to right,#fbc2eb,#a6c1ee);
}
.current{
    background-color: rgba(0, 0, 0, .1)
    
}
.bolck{
    display: block;
    width: 100%;
    
}
.marginTop{
    margin-top: 17px;
}
</style>