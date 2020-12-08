<template>
  <section class="forget-sec-1">
    <div class="container">
      <form method="post" @submit.prevent="forgot" ref="formForget">
      <div class="login-inner-wrap">
        <div class="login-header">
          <h3>Forgot Password</h3>
        </div>
        <div class="login-body">
          <div class="login-single-field">
            <label>Email:</label>
            <input type="email" placeholder="Enter Email" name="email" v-model="email">
          </div>
        </div>
        <div class="login-footer">
          <button>Send</button>
        </div>
      </div>
      </form>
    </div>
  </section>
</template>

<script>
export default {
  name: 'ForgotPassword',
  data(){
    return{
      axios: require("axios"),
      email: '',
    }
  },
  methods: {
    forgot(){
      const data = new FormData(this.$refs.formForget);
      this.axios.post(process.env.VUE_APP_APIURL + 'forgot_password_api', data)
      .then((response) => {
        const status = response.data.Status;
        console.log(response);
        console.log(status);
        if(status == '200'){
            alert('Password rest link is sent to your email.');
            this.$router.push('/');
          }
          // else if(status == '400') {
          //   alert('Bad Request. Server issue occured.')
          // }
          // else if(status == '403') {
          //   alert(response.data.Status_Detail)
          // }
          else{
            alert('oinvalid')
          }
      })
      .catch((error) => {
        console.log('sasasas', error);
        if(error.response.status == '403'){
          alert("Invalid Email! Please enter email with you've already signed up")
        }
      })
      this.email= '';
    }
  }
}
</script>

<style scoped>
.forget-sec-1{
  padding: 60px 0;
}
.login-inner-wrap{
  background: #f4f4f4;
  max-width: 450px;
  margin: 0 auto;
  padding: 30px;
  border-radius: 5px;
  display: flex;
  flex-wrap: wrap;
  flex-direction: column;
  justify-content: center;
  box-shadow: 0 0 6px rgba(2, 3, 3, .1);
}
.login-header{}
.login-header h3{
  text-align: center;
  font-size: 22px;
  color: #000;
  text-transform: uppercase;
  font-weight: 600;
  line-height: 1.2;
  margin-bottom: 5px;
}
.login-body{}
.login-single-field{
  margin-bottom: 10px;
}
.login-single-field label{
  color: #000;
  font-size: 14px;
  line-height: 1.2;
  margin-bottom: 5px;
  font-weight: 500;
  font-family: Poppins,sans-serif;
}
.login-single-field input{
  width: 100%;
  background: #fff;
  border: none;
  height: 35px;
  padding: 0 10px;
  box-shadow: 0 0 6px rgba(2, 3, 3, .1);
  font-size: 14px;
  color: #000;
  font-family: Poppins,sans-serif;
}
.login-footer{
  text-align: center;
  margin-top: 30px;
}
.login-footer button{
  width: 145px;
  background-image: linear-gradient(0deg, #ac0000, #c00000, #d40000, #e80000, #fd0000);
  color: #fff;
  font-family: Poppins, sans-serif;
  font-size: 16px;
  line-height: 26px;
  font-weight: 700;
  height: 37px;
  border: none;
}
</style>