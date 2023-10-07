---
tags:
    - 其它
    - PHP
---

## **前言**

WP-Cron 或 WordPress cron 是 WordPress 程序内置的计划任务系统，用于处理定时任务计划。WordPress 默认执行许多计划任务，其中包括：

- WordPress 核心更新检查
- 插件更新检查
- 主题更新检查
- 发布预定帖子

一些第三方插件还会利用 WP-Cron 安排其他计划任务，例如，定时自动备份数据库或网站文件。因此，我们必须确保 WP-Cron 能够正常运行。

WP-Cron 的名称来自 cron，cron 是类 Unix 系统中的基于时间的作业调度程序。但是，由于 WordPress 的要求很少，因此 WP-Cron 可以在任何托管服务提供商（包括共享虚拟主机）上工作，而无需使用任何外部软件或工具来安排事件。

为了使 WP-Cron 在不使用外部软件的情况下安排任务，必须在 WordPress 请求的生命周期内检查 cron 计划。该流程如下所示：

1. 在 `init` 挂接过程中，WP-Cron 会检查数据库中的计划事件
2. 如果计划的事件到期，则将 HTTP 请求生成到 wp-cron.php 文件
3. wp-cron.php 处理与计划任务相关的钩子

虽然 WP-Cron 使得可以在 WordPress 中安排事件成为可能，但这并不是没有问题的。

### **WP-Cron 并不可靠**

为了使 WP-Cron 触发计划的事件，WordPress 网站必须接收常规流量。对于低流量站点，这可能会造成问题，并且如果长时间未收到任何流量，则会导致错过计划的事件。
同样，大量页面缓存的高流量站点也可能遇到相同的调度问题。页面缓存实际上将WordPress 变成了静态 HTML 站点，这意味着请求不再执行 WordPress，也无法再触发计划的事件。

### **WP-Cron 效率低下**

对于无法进行页面缓存的高流量站点，WP-Cron 的效率极低。这是因为 WP-Cron将在每次页面加载时检查计划的事件。这可能意味着每秒检查一次 cron 计划。大多数计划的事件不需要这种粒度级别，因为计划的事件通常每分钟运行一次。因此，不需要在每个页面加载时检查计划的事件。

一个普遍的误解是，每个页面加载都会导致对 wp-cron.php 的 HTTP 请求，但是值得庆幸的是事实并非如此。WordPress 有一个基本的锁定系统，该系统试图防止在 60 秒内多次调用 wp-cron.php。

每当向 wp-cron.php 发出请求时，都会创建一个锁（存储为 WordPress 瞬态），该锁存储当前的 Unix 时间戳。在 60 秒过去之前，WordPress 将不再对 wp-cron.php 产生其他请求。该系统确实引入了一定程度的原子性，但这并不是万无一失的。接收到多个同时请求的高流量站点仍可以产生对 wp-cron.php 的多个请求，从而增加服务器负载。

### **如何禁用 WP-Cron**

要禁用 WP-Cron ，请将以下指令添加到 WordPress 网站根目录的 `wp-config.php` 文件中。

```
define( 'DISABLE_WP_CRON', true );
```

这将防止 WP-Cron 在每次页面加载时自动检查计划的 cron 事件。这不会完全禁用 WordPress 中的计划事件，而只会自动检查和触发计划事件。为了确保预定事件继续工作，您需要一种替代方法来触发预定事件。

### **添加 Linux 计划任务**

在类似 Unix 的系统中，cron 是在后台运行的系统守护程序。与 WP-Cron 不同， Unix cron 连续运行并使用系统时钟安排任务。这意味着永远不会错过预定的事件。

在 Linux 系统的 crontab 中填加以下内容 (请修改对应的 WordPress 目录) ，设置每 10 分钟访问一次 wp-cron.php 以执行 WordPress 定时任务。

```
*/10 * * * * wget -q -O - https://www.otakusay.com/wp-cron.php?doing_wp_cron &>/dev/null
```

或者

```
*/10 * * * * curl http://www.otakusay.com/wp-cron.php?doing_wp_cron &>/dev/null
```

又或者

```
*/10 * * * * cd /www/wwwroot/www.otakusay.com; php wp-cron.php &>/dev/null
```

对于宝塔面板，可以在后台计划任务中添加一个每隔 10 分钟运行一次的 Shell 脚本。(注意修改 WordPress 网站路径)

```
cd /www/wwwroot/www.otakusay.com; php wp-cron.php &>/dev/null
```

![WordPress 网站优化降低 CPU 占用先从 WP-Cron 计划任务开始 - 御宅说](/img-post/开发/其它/PHP/WordPress 的WP-Cron 计划任务改成crontab.assets/20201109090724910.png)宝塔面板添加 WordPress 计划任务

## **后语**

我们通过宝塔面板后台的计划任务替代了 WordPress 内置的前端触发类型的 cron 定时任务，Linux 系统基于时间的计划任务更稳定，不会因为前台访客少从而错过执行计划任务的时间，也不会因为前台访客多而过于频繁的执行 WordPress 计划任务。一般而言绝大多数的网站 10 分钟一次的计划任务都足够了，当然你也可以缩短这个触发时间。



https://www.otakusay.com/601.html