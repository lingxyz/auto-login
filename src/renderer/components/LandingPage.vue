<template lang="pug">
#wrapper
  //- img(id="logo" src="~@/assets/logo.png" alt="electron-vue")
  h1 AUTO-LOGIN
  main
    el-form(ref='form', :model='form', label-width='80px')
      el-form-item(label='请求地址')
        el-input(v-model='form.url')
      el-form-item(label='请求头')
        el-input(type='textarea', v-model='form.headers')
      el-form-item(label='请求方式')
        el-select(v-model='form.method', placeholder='请选择请求方式')
          el-option(label='GET', value='GET')
          el-option(label='POST', value='POST')
      el-form-item(label='请求参数' v-if="form.method === 'GET'")
        el-input(type='textarea', v-model='form.params')
      el-form-item(label='请求体' v-if="form.method === 'POST'")
        el-input(type='textarea', v-model='form.data')
      el-form-item(label='自动登录')
        el-switch(v-model='form.autoLogin')

      el-form-item
        el-button(type='primary', @click='onSubmit') 立即登录
        el-button(@click="$router.push('/script')") 切换至脚本
    .response {{form.response}}
</template>

<script>
  import { Loading } from 'element-ui'
  import qs from 'qs'

  export default {
    name: 'landing-page',
    data () {
      return {
        form: {
          url: localStorage.url || '', // axios 请求的url
          headers: localStorage.headers || '', // axios 请求的 headers
          method: localStorage.method || 'GET', // axios 请求的方式，默认 GET
          params: localStorage.params || '', // type = GET 时，axios 请求的 params
          data: localStorage.data || {}, // type = POST 时，axios 请求的 body
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
        localStorage.url = this.form.url
        localStorage.headers = this.form.headers
        localStorage.method = this.form.method
        localStorage.params = this.form.params
        localStorage.data = this.form.data
        localStorage.autoLogin = JSON.stringify(this.form.autoLogin)

        // ajax 提交
        let loadingInstance
        this.form.response = await this.$http({
          method: 'post',
          url: this.form.url,
          headers: this.evil(this.form.headers),
          // `transformRequest` 允许在向服务器发送前，修改请求数据
          transformRequest: [function (data) {
            loadingInstance = Loading.service({text: '登录中...'})
            return data
          }],
          // `transformResponse` 在传递给 then/catch 前，允许修改响应数据
          transformResponse: [function (data) {
            loadingInstance.close()
            return data
          }],
          data: qs.stringify(this.evil(this.form.data)),
          params: this.evil(this.form.params)
        })
      },

      // 清空数据
      // clearField () {
      //   console.log(this.$refs.form)
      //   this.$refs['form'].resetFields()
      //   localStorage.clear()
      // },

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
// #logo
//   height auto
//   margin-bottom 20px
//   width 420px
h1
  color #64b587
main
  display flex
  justify-content space-between
  & > div
    flex-basis 50%
</style>