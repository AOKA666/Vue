<template>
	<div class="box">
		<img src="../../static/image/Loginbg.3377d0c.jpg" alt="">
		<div class="register">
			<div class="register_box">
        <div class="register-title">注册路飞学城</div>
				<div class="inp">
					<input v-model = "telephone" type="text" placeholder="手机号码" class="user">
					<div id="geetest"></div>
					<input v-model = "sms" type="text" placeholder="输入验证码" class="user">
          <span class="acquire-code" :class="inactive" @click="send_sms()">{{ sms_text }}</span>
					<button class="register_btn" @click="registerHandler()">注册</button>
					<p class="go_login" >已有账号 <router-link to="/user/login">直接登录</router-link></p>
				</div>
			</div>
		</div>
	</div>
</template>

<script>
export default {
  name: 'Register',
  data(){
    return {
        sms: "",
        telephone: "",
        is_sent_sms: false,
        sms_text: "获取验证码",
        inactive: ""
    }
  },
  created(){
  },
  methods:{
    send_sms(){
      let self = this;
      if(! /1[3-9]\d{9}/.test(this.telephone)){
          this.$message.error("手机号码格式不正确！");
          return false;
      }
      if (this.is_sent_sms){
        this.$message.error("60秒内无需重复发送！");
        return false;
      }
      this.$axios.get(`${this.$settings.HOST}/user/send/sms/`, {
          params:{telephone:this.telephone}
        }
      ).then(response=>{
        if (response.data.status){
          this.$notify({
            title: '成功',
            message: '短信已发送',
            type: 'success'
          });
          this.is_sent_sms = true;
          let interval_time = 60;
          let self = this;
          let timer = setInterval(function(){            
            if(interval_time<1){
              clearInterval(timer);
              self.inactive = "";
              self.sms_text = "获取验证码"
              self.is_sent_sms = false;
            }else{
              interval_time--;
              self.inactive = "inactive";
              self.sms_text = `${interval_time}秒后重新获取`;
            }                       
          }, 1000)
            
        }
      }).catch(error=>{
        console.log(error);
        self.$notify.error({
          title: '错误',
          message: error.response.data.msg
        });
      })
    },

    registerHandler(){
      if (!this.telephone){
        this.$message.error('请输入手机号！');
        return false;
      }
      if (!this.sms){
        this.$message.error('请输入验证码！');
        return false;
      }
      this.$axios.post(`${this.$settings.HOST}/user/register/`,{
        telephone: this.telephone,
        sms: this.sms
      }).then(response=>{
        console.log(response.data);
        // let jwt = require("jsonwebtoken");
        // let token = jwt.decode(response.data.data.access)
        // sessionStorage.clear();
        localStorage.token = response.data.data.access.access; // what the f**k is this
        localStorage.id = response.data.data.id;
        localStorage.username = response.data.data.username;
        // 页面跳转
        let self = this;
          this.$alert("登录成功!", "路飞学城", {
            callback() {
              self.$router.push("/");
            }
          });
      }).catch(error=>{
        console.log(error.response);
        this.$notify.error({
          title: '错误',
          message: error.response.data.msg.non_field_errors[0]
        });
      })
    }

  },

};
</script>

<style scoped>
.box{
	width: 100%;
  height: 100%;
	position: relative;
  overflow: hidden;
}
.box img{
	width: 100%;
  min-height: 100%;
}
.box .register {
	position: absolute;
	width: 500px;
	height: 400px;
	left: 0;
  margin: auto;
  right: 0;
  bottom: 0;
  top: -338px;
}
.register .register-title{
    width: 100%;
    font-size: 24px;
    text-align: center;
    padding-top: 30px;
    padding-bottom: 30px;
    color: #4a4a4a;
    letter-spacing: .39px;
}
.register-title img{
    width: 190px;
    height: auto;
}
.register-title p{
    font-family: PingFangSC-Regular;
    font-size: 18px;
    color: #fff;
    letter-spacing: .29px;
    padding-top: 10px;
    padding-bottom: 50px;
}
.register_box{
    width: 400px;
    height: auto;
    background: #fff;
    box-shadow: 0 2px 4px 0 rgba(0,0,0,.5);
    border-radius: 4px;
    margin: 0 auto;
    padding-bottom: 40px;
}
.register_box .title{
	font-size: 20px;
	color: #9b9b9b;
	letter-spacing: .32px;
	border-bottom: 1px solid #e6e6e6;
	 display: flex;
    	justify-content: space-around;
    	padding: 50px 60px 0 60px;
    	margin-bottom: 20px;
    	cursor: pointer;
}
.register_box .title span:nth-of-type(1){
	color: #4a4a4a;
    	border-bottom: 2px solid #84cc39;
}

.inp{
	width: 350px;
	margin: 0 auto;
}
.inp input{
    outline: 0;
    width: 100%;
    height: 45px;
    border-radius: 4px;
    border: 1px solid #d9d9d9;
    text-indent: 20px;
    font-size: 14px;
    background: #fff !important;
}
.inp input.user{
    margin-bottom: 16px;
}
.inp .rember{
     display: flex;
    justify-content: space-between;
    align-items: center;
    position: relative;
    margin-top: 10px;
}
.inp .rember p:first-of-type{
    font-size: 12px;
    color: #4a4a4a;
    letter-spacing: .19px;
    margin-left: 22px;
    display: -ms-flexbox;
    display: flex;
    -ms-flex-align: center;
    align-items: center;
    /*position: relative;*/
}
.inp .rember p:nth-of-type(2){
    font-size: 14px;
    color: #9b9b9b;
    letter-spacing: .19px;
    cursor: pointer;
}

.inp .rember input{
    outline: 0;
    width: 30px;
    height: 45px;
    border-radius: 4px;
    border: 1px solid #d9d9d9;
    text-indent: 20px;
    font-size: 14px;
    background: #fff !important;
}

.inp .rember p span{
    display: inline-block;
  font-size: 12px;
  width: 100px;
  /*position: absolute;*/
/*left: 20px;*/

}
#geetest{
	margin-top: 20px;
}
.register_btn{
     width: 100%;
    height: 45px;
    background: #84cc39;
    border-radius: 5px;
    font-size: 16px;
    color: #fff;
    letter-spacing: .26px;
    margin-top: 30px;
}
.inp .go_login{
    text-align: center;
    font-size: 14px;
    color: #9b9b9b;
    letter-spacing: .26px;
    padding-top: 20px;
}
.inp .go_login span{
    color: #84cc39;
    cursor: pointer;
}
  .acquire-code{
    position: absolute;
    top: 187px;
    right: 90px;
    border-left: 2px solid #ffc210;
    padding-left: 10px;
    cursor: pointer;
    width: 120px;
    text-align: center;
  }
  .acquire-code.inactive{
    color:#999;
    cursor: default;
  }
</style>
