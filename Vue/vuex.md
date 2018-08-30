### vuex

- 目录格式

  ```
  store
  │   store.js   
  │
  └───modules
     │   user.js
     │
     │   cart.js
  
  ```

- store.js

  ```js
  import Vue from 'vue'
  import Vuex from 'vuex'
  import user from './modules/user'
  import createLogger from 'vuex/dist/logger'
  
  Vue.use(Vuex)
  
  const debug = process.env.NODE_ENV !== 'production'
  
  export default new Vuex.Store({
    modules: {
      user
    },
    strict: debug,
    plugins: debug ? [createLogger()] : []
  })
  ```

- modules/user.js

  ```js
  // initial state
  // shape: [{ id, quantity }]
  const state = {
    isLand: false,
  }
  
  // getters
  const getters = {
  
  }
  
  // actions
  const actions = {
    levelAdd({
      commit,
      state
    },level) {
      commit('levelGet', level)
    },
  }
  
  // mutations
  const mutations = {
    levelGet(state, level) {
      state.isLand = level
    },
  }
  
  export default {
    namespaced: true,
    state,
    getters,
    actions,
    mutations
  }
  
  ```


- 调用

  - mapActions

    ```js
    methods: {
      ...mapActions([
        'foo',
        'bar'
      ])
    }
    ```

    相当于

    ```js
    methods: {
      foo(val){
        return this.$store.dispatch('some/nested/module/foo', val))
      }
    }
    ```

  - mapState

    ```js
    
    computed: {
                ...mapState([
                    'user',
                    'cart'
                ]),
            }, 
    mounted(){  
                console.log(this.user)
            }
    ```

    直接获取

    ```js
    this.this.$store.state.user
    ```
