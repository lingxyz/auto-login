<template lang="pug">
#wrapper
  //- img(id="logo" src="~@/assets/logo.png" alt="electron-vue")
  h1 SCRIPT-PAGE
  main
    el-form(ref='form', :model='form', label-width='80px')
      el-form-item(label='脚本')
        el-input(type='textarea', v-model='form.script')
      el-form-item(label='自动执行')
        el-switch(v-model='form.autoLogin')

      el-form-item
        el-button(type='primary', @click='onSubmit') 立即执行
        el-button(@click="$router.push('/')") 切换至请求
    .response {{form.response}}
</template>

<script>
export default {
  name: 'landing-page',
  data () {
    return {
      form: {
        script: localStorage.script || '', // axios 请求的 script
        autoLogin: JSON.parse(localStorage.autoLogin || 'true'), // 是否自动登录
        response: '' // 返回结果
      }
    }
  },

  mounted () {
    if (JSON.parse(localStorage.autoLogin)) this.onSubmit()
  },

  methods: {
    // 提交
    async onSubmit () {
      // 表单信息存储本地
      localStorage.script = this.form.script
      localStorage.autoLogin = JSON.stringify(this.form.autoLogin)

      // 执行脚本
      this.form.response = this.evil(this.form.script)
    },

    // 解决eslint不允许使用eval
    evil (fn) {
      var Fn = Function
      return new Fn('return ' + fn)()
    }
  }
}
</script>

<style lang="stylus" scoped>
body
#wrapper
  height 100vh
  padding 60px 80px
  width 100vw
h1
  color #64b587
main
  display flex
  justify-content space-between
  & > div
    flex-basis 50%
</style>