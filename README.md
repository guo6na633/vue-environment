# vue-environment
区分vue不同环境配置
## (一)vue-cli2.0不同环境区分
### 1.安装cross-env
- npm i cross-env --save-dev
### 2.package.json 修改打包命令
 - "build-uat": "cross-env NODE_ENV=uat ENV_CONFIG=prod node build/build.js"
 NODE_ENV为不同域名环境  ENV_CONFIG为打包环境
### 3.config中添加环境test.env.js
 ```
 module.exports = {
  NODE_ENV: '"uat"',
  ENV_CONFIG: '"prod"',
  API_ADDRESS: '"https://testxxx.com"',
}
 ```
### 4.修改config中dev.env.js
 ```
  const merge = require('webpack-merge')
const testEnv = require('./test.env')
module.exports = merge(testEnv, {
  NODE_ENV: '"development"',
  ENV_CONFIG: '"dev"',
  API_ADDRESS: '""',
})
const testEnv = require('./test.env')
 ```
### 5.修改build中utils.js
 ```
exports.assetsPath = function (_path) {
  const assetsSubDirectory = process.env.ENV_CONFIG === 'prod'
    ? config.build.assetsSubDirectory
    : config.dev.assetsSubDirectory
  return path.posix.join(assetsSubDirectory, _path)
}
 ```
###6.修改build中webpack.base.conf.js 
 ```
  output: {
    path: config.build.assetsRoot,
    filename: '[name].js',
    publicPath: process.env.ENV_CONFIG === 'prod'
      ? config.build.assetsPublicPath
      : config.dev.assetsPublicPath
  }
 ```
 ###7.修改build中webpack.prod.conf.js 
 ```
const env = require('../config/'+process.env.NODE_ENV+'.env')
 ```
  
  
  
  
  
  
  
  
  
  
