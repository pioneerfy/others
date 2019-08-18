<template>
  <div id="app">
   <!--<a-form :model="form" label-width="80px">
      <a-form-item style="margin-top: -10px; margin-bottom:0px;">
       <cron v-if="showCronBox" v-model="form.cronExpression"></cron>
       <span style="color: #E6A23C; font-size: 12px;">corn从左到右（用空格隔开）：秒 分 小时 月份中的日期 月份 星期中的日期 年份</span>
     </a-form-item>
     <a-form-item label="Cron">
       <a-input v-model="form.cronExpression" auto-complete="off">
          <a-button slot="addonAfter" v-if="!showCronBox" icon="up" @click="showCronBox = true" title="打开图形配置">打开图形配置</a-button>
          <a-button slot="addonAfter" v-else icon="down" @click="showCronBox = false" title="关闭图形配置">关闭图形配置</a-button>
       </a-input>
     </a-form-item>
    </a-form>-->
      <a-form id="components-form-demo-normal-login" :form="form" class="login-form" @submit="handleSubmit">
        <a-form-item v-bind="formItemLayout">
          <span slot="label">Nickname&nbsp;
            <a-tooltip title="What do you want others to call you?">
              <a-icon type="question-circle-o" />
            </a-tooltip>
          </span>
          <a-input v-decorator="[ 'nickname', { rules: [{ required: true, message: 'Please input your nickname!', whitespace: true }]}]" />
        </a-form-item>
        <a-form-item v-bind="formItemLayout" label="E-mail">
          <a-input v-decorator="[ 'email', { rules: [{ type: 'email', message: 'The input is not valid E-mail!', }, { required: true, message: 'Please input your E-mail!',}]}]"/>
        </a-form-item>
        <a-form-item v-bind="formItemLayout" label="Password">
          <a-input v-decorator="[ 'password', { rules: [{ required: true, message: 'Please input your password!',}, { trigger: 'blur' }, { validator: validateToNextPassword, }],}]" type="password" />
        </a-form-item>
        <a-form-item v-bind="formItemLayout" label="Confirm Password">
          <a-input v-decorator="[ 'confirm', { rules: [{ required: true, message: 'Please confirm your password!', }, { validator: compareToFirstPassword, }], }]" type="password" @blur="handleConfirmBlur"/>
        </a-form-item>
        <a-form-item v-bind="tailFormItemLayout">
          <a-button type="primary" html-type="submit">Register</a-button>
        </a-form-item>
      </a-form>
  </div>
</template>

<script>
import cron from './components/cron'

export default {
  name: 'App',
  components: {
    cron
  },
  beforeCreate () {
    this.form = this.$form.createForm(this)
  },
  methods: {
    handleSubmit  (e) {
      e.preventDefault()
      this.form.validateFieldsAndScroll((err, values) => {
        if (!err) {
          console.log('Received values of form: ', values)
        }
      })
    },
    handleConfirmBlur  (e) {
      const value = e.target.value
      this.confirmDirty = this.confirmDirty || !!value
    },
    // 密码
    validateToNextPassword  (rule, value, callback) {
      // const form = this.form
      // if (value && this.confirmDirty) {
      //   form.validateFields(['confirm'], { force: true })
      // }
      // callback()
      const reg = /^ab/
      if (reg.test(value)) {
        callback()
      } else {
        callback(new Error('错误'))
      }
    },
    // 确认密码
    compareToFirstPassword  (rule, value, callback) {
      const form = this.form
      if (value && value !== form.getFieldValue('password')) {
        callback(new Error('Two passwords that you enter is inconsistent!'))
      } else {
        callback()
      }
    }
  },
  data () {
    return {
      // showCronBox: false,
      // form: {
      //   cronExpression: ''
      // }
      confirmDirty: false,
      formItemLayout: {
        labelCol: {
          xs: { span: 24 },
          sm: { span: 8 }
        },
        wrapperCol: {
          xs: { span: 24 },
          sm: { span: 16 }
        }
      },
      tailFormItemLayout: {
        wrapperCol: {
          xs: {
            span: 24,
            offset: 0
          },
          sm: {
            span: 16,
            offset: 8
          }
        }
      }
    }
  }
}
</script>

<style>
#app {
  font-family: 'Avenir', Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  text-align: center;
  width: 700px;
  color: #2c3e50;
  margin-top: 60px;
}
#components-form-demo-normal-login .login-form {
  max-width: 300px;
}
#components-form-demo-normal-login .login-form-forgot {
  float: right;
}
#components-form-demo-normal-login .login-form-button {
  width: 100%;
}
</style>
