### axios

- 基础配置

  ```js
  var http = axios.create({
    baseURL: 'http://api:8000/',//全局地址配置
  });
  ```

- 发送请求

  ```js
  this.$http({
     method: 'post',
     url: '/user/login/',
     data: {
       mobile: this.loginForm.data.mobile,
       password: this.loginForm.data.password
     }
   }).then(response =>{
     console.log(response)
     //设置vuex的token和用户信息
     this.$store.commit('userSet',response.data)
     this.$router.push('home')
   })
   .catch(error => {
     console.log(error);
          });
  ```

  

- 拦截器

  ```js
  // http request 拦截器
  http.interceptors.request.use(
    config => {
      if (store.state.token) { // 判断是否存在token，如果存在的话，则每个http header都加上token
        config.headers.Authorization = `token ${store.state.token}`;
      }
      return config;
    },
    err => {
      return Promise.reject(err);
    });
  ```

  