<template>
  <section class="login">
    <div style="height: 30px; -webkit-app-region: drag">
    </div>
    <div class="form">
      <el-form>
        <h2 class="title">SNS短信</h2>
        <div class="form-group">
          <el-input type="text" id="AccessKey" v-model="phone" placeholder="11位手机号码" clearable="true"></el-input>
        </div>
        <div class="form-group">
          <el-input type="textarea" id="SecretKey" v-model="message" placeholder="短信内容，最多140个字符"
                    :autosize="{ minRows: 3, maxRows: 6}">
          </el-input>
        </div>
        <el-button :loading="sending" type="primary" @click.prevent="sendSMS" size="small" class="btn">发送</el-button>
        <label style="color: lightcoral">{{ errorMsg }}</label>
      </el-form>
    </div>
  </section>
</template>

<script>
import AWS from 'aws-sdk'

export default {
  data () {
    return {
      phone: '',
      message: '',
      sending: false,
      errorMsg: ''
    }
  },
  methods: {
    sendSMS () {
      const self = this
      self.errorMsg = ''

      if (self.phone === '' || self.message === '') {
        self.$message.error('手机号码和短信内容不能为空')
        return
      }

      if (this.phone.length !== 11) {
        self.$message.error('必须是11位手机号')
        return
      }

      self.sending = true
      const sns = new AWS.SNS({
        region: 'ap-northeast-1'
      })
      const params = {
        Message: self.message,
        PhoneNumber: '+86' + self.phone
      }
      sns.publish(params, function (err) {
        if (err) {
          self.$message({
            message: '发送失败',
            type: 'error',
            duration: 2000,
            showClose: true
          })
          console.error(err.message)
          self.errorMsg = err.message
          self.sending = false
        } else {
          self.$message({
            message: '发送成功',
            type: 'success',
            duration: 2000,
            showClose: true
          })
          self.sending = false
        }
      })
    }
    // checkLogin() {
    //   let login = JSON.parse(localStorage.obj || "null");
    //   if (login) {
    //     this.$electron.ipcRenderer.send("status", true);
    //   }
    //   // this.$electron.ipcRenderer.send("status", true);
    // }
  },
  created () {
    // this.checkLogin();
  }
}
</script>

<style scoped>
.login {
  background: #409eff;
  width: 100%;
  height: 100%;
}
.form form {
  width: 300px;
  margin: 0 auto;
  background: #fff;
  top: 50%;
  left: 50%;
  position: absolute;
  transform: translate(-50%, -50%);
  box-shadow: 0 1px 5px 0px rgba(0, 0, 0, 0.1);
  border-radius: 4px;
  padding: 30px;
}

.form form .title {
  text-align: center;
  font-size: 18px;
  line-height: 26px;
  margin-bottom: 15px;
  color: #409EFF;
}

.form-group {
  margin-bottom: 15px;
}

label {
  display: inline-block;
  max-width: 100%;
  margin-bottom: 5px;
  font-weight: 700;
}

.form-control {
  display: block;
  width: 100%;
  height: 34px;
  padding: 6px 12px;
  font-size: 14px;
  line-height: 1.42857143;
  color: #555;
  background-color: #fff;
  background-image: none;
  border: 1px solid #D7DAE2;
  border-radius: 4px;
  -webkit-transition: border-color ease-in-out 0.15s,
    -webkit-box-shadow ease-in-out 0.15s;
  -o-transition: border-color ease-in-out 0.15s, box-shadow ease-in-out 0.15s;
  transition: border-color ease-in-out 0.15s, box-shadow ease-in-out 0.15s;
}

.btn {
  width: 100%;
}
::-webkit-input-placeholder { /* WebKit browsers */
    color:    #D7DAE2;
}
:-moz-placeholder { /* Mozilla Firefox 4 to 18 */
    color:    #D7DAE2;
}
::-moz-placeholder { /* Mozilla Firefox 19+ */
    color:    #D7DAE2;
}
:-ms-input-placeholder { /* Internet Explorer 10+ */
    color:    #D7DAE2;
}
</style>
