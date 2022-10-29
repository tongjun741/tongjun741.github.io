---
tags:
    - IDE
---

IntelliJ IDEA中无法输入中文标点的解决办法

前段时间升级了IntelliJ IDEA 15 和 WebStorm 11 ，突然发现IntelliJ IDEA 15 和 WebStorm 11 中无法输入中文标点。到大猫的IntelliJ IDEA前端开发 群里问了一下，找到了解决方案：

IntelliJ IDEA 15 和 WebStorm 11 内置 JDK 1.8 的版本，但是这个版本存在中文标点输入以后自动被转换成英文（半角）的 Bug。

经试验，使用 JDK 8u45 可以正常输入中文标点，具体如下：

- 下载安装 JDK 8u45；

- 重启IntelliJ IDEA；

- 选择 JDK ：按 CMD + Shift + A，输入 JDK，选择 Switch IDE boot JDk… 可以选择新安装的JDK 8u45；

OK。

