### 关于css 3d 失效变为2d

今天在写一个3d进度条的特效，使用了以下3d代码:

```scss
{
  	perspective-origin: 50% 50%;	
  	perspective: 1000px;
    transform: rotateX(55deg);
    transform-style: preserve-3d;
}
```

在safari正常渲染，windows的chrome渲染异常

#### 第一想到的是内核导致了问题？

然后测试了其他内核

- mac
  - safari — 正常
  - chrome — 正常
  - firefox —正常
- windows
  - chrome — 3d变2d
  - firefox — 3d变2d
  - edge — 3d变2d

查阅相关资料，发现原因

> When we have mix-blend-mode, the closest ancestor that creates stacking context will isolate blending. We create a render surface at the root of this isolated group and because render surfaces don't support preserve-3d(because they render into separate FBO), we see a flattened result.

> ajuma@ suggested that this bug maybe much easier to fix after Slimming paint v2 if we can somehow disentangle transforms from layers.

概括为：

##### 当我们使用混合模式时，会重新对这个使用了混合模式的元素的根节点处创建一个独立的渲染平面，且这个平面暂时不支持3d模式，所以渲染为2d平面



#### 总结

- `mix-blend-mode`
- `background-blend-mode`
- `filter`

以上属性会导致css 3d失效

以上bug在chromium bugs已经合并到 [issue 575099](https://bugs.chromium.org/p/chromium/issues/detail?id=575099)，状态为Untriaged，短期内无法修复，在开发中应避免以上属性和3d模式的混合使用。





