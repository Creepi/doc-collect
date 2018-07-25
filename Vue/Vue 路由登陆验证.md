### Vue 路由登陆验证

- 在router列表内添加meta

  ```js
          {
            path: '/home/partnerList',
            name: 'name',
            meta: {
              requireAuth: true,//是否需要登陆验证
            },
            component: PartnerList
          },
  ```

- 在路由钩子beforeEach添加检测

  ```js
  router.beforeEach((to, from, next) => {
    if (to.meta.requireAuth) { // 判断该路由是否需要登录权限
      if (store.state.token) { // 通过vuex state获取当前的token是否存在
        next();
      } else if (window.localStorage.getItem('current_token')) {
        store.commit('localGet', {
          user: JSON.parse(localStorage.getItem('current_user')),
          token: localStorage.getItem('current_token')
        })
        next();
      } else {
        Message.warning("请先登录")
        next({
          path: '/login',
          query: {
            redirect: to.fullPath
          } // 将跳转的路由path作为参数，登录成功后跳转到该路由
        })
      }
    } else {
      next();
    }
  })
  ```

  