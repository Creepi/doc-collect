### 移动端适配方案(vw)

- 安装

  ```js
  npm i postcss-aspect-ratio-mini postcss-px-to-viewport postcss-write-svg postcss-cssnext postcss-viewport-units cssnano cssnano-preset-advanced --S
  ```

- 相关配置项

  - / postcssrc.js

    这里为1334*646的设计稿

    ```js
    module.exports = {
      "plugins": {
        "postcss-import": {},
        "postcss-url": {},
        "postcss-aspect-ratio-mini": {}, 
        "postcss-write-svg": { utf8: false }, "postcss-cssnext": {}, "postcss-px-to-viewport": { 
          viewportWidth: 1334, // (Number) The width of the viewport. 
          viewportHeight: 646, // (Number) The height of the viewport. 
          unitPrecision: 3, // (Number) The decimal numbers to allow the REM units to grow to. 
          viewportUnit: 'vw', // (String) Expected units.
          selectorBlackList: ['.ignore', '.hairlines'], // (Array) The selectors to ignore and leave as px. 
          minPixelValue: 1, // (Number) Set the minimum pixel value to replace.
          mediaQuery: false // (Boolean) Allow px to be converted in media queries. 
         },
        "postcss-viewport-units":{}, 
        "cssnano": { 
          preset: "advanced", 
          autoprefixer: false,
          "postcss-zindex": false
          }
        // to edit target browsers: use "browserslist" field in package.json
      }
    }
      
    ```


- 优雅降级

  - Index.html

  ```js
  <script src="//g.alicdn.com/fdilab/lib3rd/viewport-units-buggyfill/0.6.2/??viewport-units-buggyfill.hacks.min.js,viewport-units-buggyfill.min.js"></script><script>
      // vw兼容性处理viewport-units-buggyfill
        window.onload = function () {
          window.viewportUnitsBuggyfill.init({ hacks: window.viewportUnitsBuggyfillHacks });
          //以下代码用户测试
          // var winDPI = window.devicePixelRatio;
          // var uAgent = window.navigator.userAgent;
          // var screenHeight = window.screen.height;
          // var screenWidth = window.screen.width;
          // var winWidth = window.innerWidth;
          // var winHeight = window.innerHeight;
          // console.log("Windows DPI:" + winDPI + ";\ruAgent:" + uAgent + ";\rScreen Width:" +
          //   screenWidth + ";\rScreen Height:" + screenHeight + ";\rWindow Width:" + winWidth +
          //   ";\rWindow Height:" + winHeight)
        }
      </script>
  ```
