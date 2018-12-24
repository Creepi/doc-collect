### svg在vue中的使用



-  svg-sprite

  [svg-sprite-loader](https%3A%2F%2Fgithub.com%2Fkisenka%2Fsvg-sprite-loader) 是一个 webpack loader ，可以将多个 svg 打包

  webpack配置如下:

  ```js
        {
          test: /\.svg$/,
          loader: 'svg-sprite-loader',
          include: [resolve('src/icons')], //指定文件夹
          options: {
            symbolId: 'icon-[name]'
          }
        }
  ```



- svgo

  [svgo](https://github.com/svg/svgo)是一个基于Nodejs的SVG文件优化工具，可以去除多余的编辑器源信息，注释等等无用信息。

  ```node
  $ svgo -f ../path/to/folder/with/svg/files
  ```

- 封装

  ```vue
  // components
  <template>
    <svg :class="svgClass" aria-hidden="true">
      <use :xlink:href="iconName"/>
    </svg>
  </template>
  
  <script>
  export default {
    name: 'SvgIcon',
    props: {
      iconClass: {
        type: String,
        required: true
      },
      className: {
        type: String,
        default: ''
      }
    },
    computed: {
      iconName() {
        return `#icon-${this.iconClass}`
      },
      svgClass() {
        if (this.className) {
          return 'svg-icon ' + this.className
        } else {
          return 'svg-icon'
        }
      }
    }
  }
  </script>
  
  <style scoped>
  .svg-icon {
    width: 1em;
    height: 1em;
    vertical-align: -0.15em;
    fill: currentColor;
    overflow: hidden;
  }
  .logo-l{
    width: 128px;
    height: 50px;
  }
  .logo-s{
    width: 34px;
    height: 34px;
    margin: 8px 0 8px 18px;
  }
  </style>
  
  ```

  全局注册组件

  ```js
  import Vue from 'vue'
  import SvgIcon from '@/components/SvgIcon' // svg组件
  // register globally
  Vue.component('svg-icon', SvgIcon)
  ```


- 使用

  ```vue
  <svg-icon v-else icon-class="logo_s" class-name="logo-s" />
  ```
