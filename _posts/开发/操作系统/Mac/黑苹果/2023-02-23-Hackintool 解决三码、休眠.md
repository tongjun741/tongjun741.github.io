---
tags:
    - 操作系统
    - Mac
    - 黑苹果
---

Hackintool 解决三码、休眠

（1）用任意文本编辑器打开硬盘 EFI 分区下的 /EFI/OC/config.plist，找到 PlatformInfo 配置部分。
（2）打开 Hackintool，选择“系统”页，“序列号生成器”，点击右下角刷新按钮，执行摇号。
（3）“序列号生成器”中的前三项即为三码，按着图示的对应关系，拷贝到文本编辑器中的 config.plist。三码复制完成后保存并关闭 /EFI/OC/config.plist 文件。

摇号，产生随机三码
为了完美的休眠，我们需要修改电源配置，按下图切换到“电源”页面，点击“修复深度休眠预留空间”，输入登录密码并确认后，保证图中 hibernatemode、proximitywake 均为绿色即可：

作者：weachy
链接：https://www.jianshu.com/p/78510cfa4a64
来源：简书
著作权归作者所有。商业转载请联系作者获得授权，非商业转载请注明出处。