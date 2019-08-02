# auto-login

> An Automatic Login Desktop Software based on [electron-vue](https://github.com/SimulatedGREG/electron-vue)@[8fae476](https://github.com/SimulatedGREG/electron-vue/tree/8fae4763e9d225d3691b627e83b9e09b56f6c935)

#### 使用步骤

- 安装并打开APP
- 输入必要参数，详细参照[配置说明]
- 选择是否自动登录。若是，则下次软件启动后会进行自动登录
- 点击登录按钮进行登录操作，默认会保存信息
- 请求发出后，会在右方显示请求返回的结果

#### 配置说明

- 参数配置

|  参数   | 说明  | 默认值
|  ----  | ----  | --- |
| url  | Ajax 请求的路径 | -
| headers  | Ajax 请求的报头 | -
| method  | Ajax 请求的方式 | GET
| params  | Ajax GET 请求参数 | -
| data  | Ajax POST 请求体 | -
| autoLogin  | 是否自动登录 | true

- 开机自启动配置

todo

#### Build Setup

``` bash
# install dependencies
npm install

# serve with hot reload at localhost:9080
npm run dev

# build electron application for production
npm run build

# run unit & end-to-end tests
npm test


# lint all JS/Vue component files in `src/`
npm run lint

```