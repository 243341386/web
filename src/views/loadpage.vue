<template>
  <div class="container">
    <div class="main">
      <div class="left_part"><img src="@/assets/leftpic.png" alt="背景" width="100%"></div>
      <div class="right_log" v-if="juice == 0">
        <div class="log_title" >
          <div></div>
          <div class="sign_in"><a @click="juice = 0">登录</a></div>
          <div class="sign_up"><a @click="juice = 1"> >注册</a></div>
          <div></div>
        </div>
        <div class="tips" v-if="!page_key">账号或密码错误</div>
        <form action="">
          <div class="log_operate">
            <div class="AccountNumber">账号</div>
            <div class="username"><input v-model="userName_1" class="username_text" type="text"></div>
            <div class="passwd">密码</div>
            <div class="pwd"><input v-model="password_1" type="password" style="border-top: none;border-left: none;border-right: none;border-bottom: 0.5px solid #bbbdc1;width: 350px;height: 40px;"></div>
            <div class='sub'><input type="submit" value="登录" @click="load_click_function" style="background: #8068ff;color: white;margin-top: 40px;width: 350px;height: 50px;font-size: 20px;font-weight: 600;border: none;"></div>
            <div class="forget_pwd" style="margin-top: 30px;margin-left: 140px;"><a @click="juice = 2" style="color: #787a7c;font-weight: 100;">忘记密码？</a></div>
          </div>
        </form>
      </div>

      <div class="right_log" v-if="juice == 1">
        <div class="log_title" style="width: 500px;height: 80px;text-align: center;display: flex;justify-content: space-around;">
          <div></div>
          <div class="sign_in" style="display: inline;width:50px;height: 30px;padding-top: 10px; font-size: 17px;"><a @click="juice = 0">登录</a></div>
          <div class="sign_up" style="display: inline;width:50px;height: 30px;padding-top: 10px; border-bottom:5px solid black;font-size: 17px;"><a @click="juice = 1">注册</a></div>
          <div></div>
        </div>
        <div class="tips" style="color: red;text-align: center;height: 20px;">账号和密码只支持英文和数字字符</div>
        <form action="">
          <div class="log_operate" style="margin-left: 80px;margin-top: 30px;">
            <div style="color: #787a7c;font-size: 17px;margin-bottom: 10px;">账号</div>
            <div class="username"><input v-model="userName_2" type="text" style="border-top: none;border-left: none;border-right: none;border-bottom: 0.5px solid #bbbdc1;width: 350px;height: 40px;"></div>
            <div style="color: #787a7c;font-size: 17px;margin-top: 40px;margin-bottom: 10px;">密码</div>
            <div class="pwd"><input v-model="password_2" type="password" style="border-top: none;border-left: none;border-right: none;border-bottom: 0.5px solid #bbbdc1;width: 350px;height: 40px;"></div>
            <div class='sub'><input type="submit" value="注册" @click="reg_click_function" style="background: #8068ff;color: white;margin-top: 40px;width: 350px;height: 50px;font-size: 20px;font-weight: 600;border: none;"></div>
          </div>
        </form>
      </div>

      <div class="right_log" style="width: 600px; margin-left: 400px;" v-if="juice == 2">
        <div style="text-align: center;"><h1>修改密码</h1></div>
        <div style="text-align: center;font-size: 28px;margin-top: 50px;"><p>非常抱歉因为一些原因，暂不支持自助修改密码</p></div>
        <div style="text-align: center;font-size: 19px;"><a><p>如需帮助，请联系QQ：xxxxxxxxx</p></a></div>
      </div>
    </div>

  </div>
</template>

<script>
  export default {
    name: "loadpage",
    data(){
      return {
        page_key: true,
        juice: 0,  // 0-注册，1-登陆，2-忘记密码
        userName_1:"",
        password_1:"",
        userName_2:"",
        password_2:"",
      }
    },
    mounted(){
      this.$api.get('api/main/pub/login', {
        topCount: 8
      }, response =>{
        if (response.status >= 200 && response.status < 300) {
          console.log(response.data);
        } else {
          console.log(response.message);
        }
      })
    },
    methods:{
      load_click_function(){
        //这里写登陆逻辑，先做验证再写接口，接口写在这里面，
         if(this.userName_1 && this.password_1){
           this.$api.get('api/work/pub/login', {
             "loginName": this.userName_1,
             "password": this.password_1,
             "rememberMe": true
           }, response =>{
             if (response.status >= 200 && response.status < 300) {
               console.log(response.data);
             } else {
               this.page_key = !this.page_key;
             }
           })
         }else{
           this.page_key = !this.page_key;
         }
      },
      reg_click_function(){
        //这里写注册逻辑，先做验证再写接口，接口写在这里面，
        if(this.userName_2 && this.password_2){
          this.$api.get('api/work/pub/register', {
            "headImage": this.userName_2,
            "pseudonym": this.password_2,
          }, response =>{
            if (response.status >= 200 && response.status < 300) {
              console.log(response.data);
            } else {
              this.page_key = !this.page_key;
            }
          })
        }else{
          this.page_key = !this.page_key;
        }
      }
    }
  }

</script>

<style scoped>
  .sign_in{
    /*display: inline;*/
    width:50px;
    height: 30px;
    padding-top: 10px;
    border-bottom:5px solid black;
    font-size: 17px;
  }
  .log_title{
    width: 500px;
    height: 80px;
    text-align: center;
    display: flex;
    justify-content: space-around;
  }
  .container{
    max-width: 1920px;
    max-height: 1080px;
    margin: 0 auto;
    padding: 0 auto;

  }

  .main{
    height: 1000px;
    background-color: #ffffff;
    display: flex;
  }

  .nav{
    height:68px;
    max-width: 100%;
    background:#ffffff;
    display: flex;
    justify-content: space-around;
  }

  .nav div{
    width: 100px;
    padding-top: 30px;
  }

  .left_part{
    margin-top:8%;
    margin-left:8%;

  }

  .right_log{
    width: 500px;
    height: 500px;
    background: #ffffff;
    margin-top:12%;
    margin-left:27%;
  }

  /* .sign_in::after{
      content: '';
      position: absolute;
      left: 18px;
      top: auto;
      bottom: 0;
      right: auto;
      height: 6px;
      width: 105px;

  } */

  a{
    text-decoration: none;
    color: black;
    font-weight: bold;
  }

  body::-webkit-scrollbar{
    width:7px;
  }

  body::-webkit-scrollbar-track{
    background: #ffffff;
    border-radius:2px;
  }

  body::-webkit-scrollbar-thumb{
    background: #7f7f7f;
    border-radius:10px;
  }

  body::-webkit-scrollbar-thumb:hover{
    overflow: scroll;
  }
  .sign_up{
    display: inline;
    width:50px;
    height: 30px;
    padding-top: 10px;
    font-size: 17px;
  }
  .tips{
    color: red;
    text-align: center;
    height: 20px;
  }
  .log_operate{
    margin-left: 80px;
    margin-top: 30px;
  }
  .AccountNumber{color: #787a7c;font-size: 17px;margin-bottom: 10px;}
  .username_text{border-top: none;border-left: none;border-right: none;border-bottom: 0.5px solid #bbbdc1;width: 350px;height: 40px;}
  .passwd{color: #787a7c;font-size: 17px;margin-top: 40px;margin-bottom: 10px;}
</style>
