---
title: 前端无感刷新token
tags:
categories:
  - 性能优化
---

## 目前存在的问题

token因为考虑到安全性问题（token关系着用户的个人信息等相关资料），设置有一定时间的生命周期。当用户在已经登录后出现token过期，发送
后端请求会返回401状态码，一般情况前端会重定向路由到登录页让用户重新登录。但是这样体验不佳。比如你刚填写完表单一大堆信息的情况下，点击提交按钮，
但是因为token过期了，页面跳转到登录页，就是信息白填了。。。。为了优化这个体验，refreshToken就诞生了

## 什么是无感刷新token

两种token

- requestToken
  这个token是关系这用户个人信息的token,设置在请求头中，用于鉴权，一般设置比较短的生命周期
- refreshToken
  这个token是为了更新过期的requestToken,一般会设置较长的生命周期，如果是数据都保存在服务器的话可以设置永不过期

当token过期，也就是出现状态码401时，可以调用通过refreshToken发送请求更新requestToken，然后再次发送刚才出现401状态码的请求，不需要用户再次重新登录

## 怎么做

- 在拦截器响应那里处理出现401状态码的请求

```js
// 设置一个变量isRefreshing，当出现多个并发请求时，只需要一个请求去发送刷新token的请求就满足了
let isRefreshing = false
// 设置一个数据用于保存没有发送请求成功的请求，再获取到新token时再更新token重新发送
let requests = []
axios.interceptors.response.use((res) => {
  return Promise.resolve(res.data)
},(error) => {
  let { data, config } = error.response
  if(data.status === 401){
    handle401(error)
  }
})

const handle401 = (error) => {
  let { data, config } = error.response
  if(config.url === '/api/token/refreshToken' || Number(data.errorCode) !== 1107){
    //这里处理refreshToken也失效了的情况，或者不是 token 过期的情况，可能是token无效或者异地登录之类的，不需要刷新token，直接退出到登录页
    window.location.href = 'XXX/login'
  }else{
    // 保存下来失败的请求
    const promise = new Promise((resolve, reject) => {
      requests.push(async () => {
        try {
          const rs = await axios(config);
          resolve(rs)
        } catch (e) {
          reject(e)
        }
      })
    })
    if(isRefreshing){
      return promise
    }else{
      isRefreshing = true
      // 发送刷新token请求
      const res = await axios({
      url: refreshTokenAPI,
      method: 'get',
      headers: {
        'X-Authorization': refreshToken,
      }
      })

    if (Number(res.errorCode) !== 1000) {
      throw new Error('refresh failed')
    }

    const newToken = res.data;

    cookie.set('token', newToken, {
      domain: syncUserInfo.getDomain(),
      expires: 365,
      sameSite: 'strict',
    })
    requests.forEach(cb => cb())
    requests = []
    }
  }
}
```
