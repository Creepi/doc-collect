###vue 表单初始化

通过assign复制对象

- Data

```js
        passData: {
          visible: false,
          defaultPass:{},
          form: {
            old_password: '',
            new_password1: '',
            new_password2: ''
          }
        }
```

- created

  ```js
      created(){
        this.passData.defaultPass = JSON.parse(JSON.stringify(this.passData.form));
      },
  ```

  

- method

  ```js
       this.passData.form = Object.assign(this.passData.form, 		       this.passData.defaultPass);
       this.passData.visible = false
  ```

  