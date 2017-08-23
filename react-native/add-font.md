---
title: react-native 如何添加自定义图标
---

### 首先下载图标文件

 - [阿里图标库](http://www.iconfont.cn/)

下载完毕后，只需要 `iconfont.ttf` 这个文件就行了。

### IOS配置

- 首先建立一个 `font` 文件夹,里面放入 `iconfont.ttf`
- 打开 `toydb.xcodeproj` 文件

![alt text][./img/font.jpg]

![alt text][./img/font2.png]

![alt text][./img/font3.jpg]

### android 配置

直接把 `iconfont.ttf` 放在 `项目根目录/android/app/src/main/assets/fonts/` 中就行了

### 下一步使用

- 首先项目需要重新打包 `react-native run-ios`
- 然后，添加如下代码就OK了

```html
<Text style={{fontFamily:'iconfont'}}>&#xe603;</Text>
```

> 图标字符可以点击下载的图标文件 `demo_unicode.html` 来查看
>
