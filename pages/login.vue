<template>
  <b-container class="bv-example-row">
    <b-row style="margin: 150px;">
      <b-col></b-col>
      <b-col cols="8">
        <b-tabs>
          <b-tab title="登入" active class="p5">
            <b-form @submit="onLogin">
              <b-form-group id="emailGroup" label="Email:" label-for="login-email">
                <b-form-input id="login-email" type="email" v-model="form.email" required placeholder="輸入e-mail">
                </b-form-input>
              </b-form-group>
              <b-form-group id="pwdGroup" label="密碼:" label-for="login-pwd">
                <b-form-input id="login-pwd" type="password" v-model="form.pwd" required placeholder="輸入密碼">
                </b-form-input>
              </b-form-group>
              <div class="text-right">
                <b-button type="submit" variant="primary">登入</b-button>
              </div>
              
            </b-form>
          </b-tab>
          <b-tab title="註冊" class="p5">
            <b-form @submit="onRegister" @reset="onReset">
              <b-form-group id="emailGroup" label="Email:" label-for="register-email">
                <b-form-input id="register-email" type="email" v-model="form.email" required placeholder="輸入e-mail">
                </b-form-input>
              </b-form-group>
              <b-form-group id="pwdGroup" label="密碼:" label-for="register-pwd">
                <b-form-input id="register-pwd" type="password" v-model="form.pwd" required placeholder="輸入密碼">
                </b-form-input>
              </b-form-group>
              <b-form-group id="confirmPwdGroup" label="再次確認密碼:" label-for="confirmPwd">
                <b-form-input id="confirmPwd" type="password" v-model="form.confirmPwd" required placeholder="再次確認密碼">
                </b-form-input>
              </b-form-group>
              <b-form-group id="genderGroup" label="性別">
                <b-form-radio-group id="radios1" v-model="form.gender" :options="options">
                </b-form-radio-group>
              </b-form-group>
              <div class="text-right">
                <b-button type="submit" variant="primary">註冊</b-button>
                <b-button type="reset" variant="danger">清空</b-button>
              </div>
            </b-form>
          </b-tab>
        </b-tabs>
        
      </b-col>
      <b-col></b-col>
    </b-row>
  </b-container>
</template>

<script>
  export default {
    data() {
      return {
        url:'http://localhost:4000',
        form: {
          email: '',
          pwd: '',
          confirmPwd: '',
          gender: 1
        },
        options:[
          { text: '爹地', value: 1 },
          { text: '甜心', value: 0 }
        ]
      }
    },
    methods: {
      async onLogin (evt) {
        evt.preventDefault()
        let data = {
          email: this.form.email,
          password: this.form.pwd,
          confirmPwd: this.form.confirmPwd,
          gender: this.form.gender
        }
        let res = await this.$axios.$post(this.url+'/api/users', data)
        console.log(res)
        alert(JSON.stringify(this.form))
      },
      async onRegister (evt) {
        evt.preventDefault();
        let data = {
          email: this.form.email,
          password: this.form.pwd,
          confirmPwd: this.form.confirmPwd,
          gender: this.form.gender
        }
        let res = await this.$axios.$post(this.url+'/api/users', data)
        console.log(res)
        alert(JSON.stringify(this.form));
      },
      onReset(evt) {
        evt.preventDefault();
        this.form.email = ''
        this.form.pwd = ''
        this.form.confirmPwd = ''
      }
    }
  }

</script>
<style lang="css" scoped>
.p5 {
  padding: 5px;
}
.text-right {
  text-align: right;
}
</style>
