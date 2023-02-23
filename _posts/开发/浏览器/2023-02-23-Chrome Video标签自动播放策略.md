---
tags:
    - 浏览器
---

Chrome Video标签自动播放策略

在Video标签上增加autoplay和muted属性

![](/img-post/开发/浏览器/Chrome Video标签自动播放策略.assets/d760194025b245b5891ccaca73ee8d3e.png)



https://developers.google.com/web/updates/2017/09/autoplay-policy-changes

Chrome's autoplay policies are simple:

- Muted autoplay is always allowed.

- Autoplay with sound is allowed if:

- User has interacted with the domain (click, tap, etc.).

- On desktop, the user's Media Engagement Index threshold has been crossed, meaning the user has previously played video with sound.

- The user has added the site to their home screen on mobile or installed the PWA on desktop.

- Top frames can delegate autoplay permission to their iframes to allow autoplay with sound.



https://blog.csdn.net/jacke121/article/details/88803182

