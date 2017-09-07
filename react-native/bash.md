---
title: 常用的bash命令
---

- 升级react-native `react-native-git-upgrade`,直接升级到最新版本。升级到指定的版本 `react-native-git-upgrade X.Y.Z`。
  - 升级时遇到的[问题](https://github.com/facebook/react-native/issues/11578) ,删除掉 `"react-native-camera": "git+https://github.com/lwansbrough/react-native-camera.git",` 更新完成后，再安装 `react-native-camera`
- `npm outdated` 查看有哪些包可更新
- `npm-check -u` 查看有哪些包可更新
- `npm list -g --depth 0` 查看全局安装的包(`depth 0` 是只显示最顶层的包，不显示下面的依赖包)
- `npm update` 升级所有更新包,npm 2.6.1后才支持
- `yarn upgrade react-native-modalbox` 升级包
- `which react-native` 查看包的路径
