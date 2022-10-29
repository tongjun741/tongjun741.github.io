---
tags:
    - NodeJS
---

Jenkins无法显示Mocha Report

原因是Jenkins自动增加了http header：

```javascript
content-security-policy: sandbox; default-src 'none'; img-src 'self'; style-src 'self';

```



去除这个header的方式：

The default rule is set to:

sandbox; default-src 'none'; img-src 'self'; style-src 'self';

This rule set results in the following:

No JavaScript allowed at all

No plugins (object/embed) allowed

No inline CSS, or CSS from other sites allowed

No images from other sites allowed

No frames allowed

No web fonts allowed

No XHR/AJAX allowed, etc.

To relax this rule, go to Manage Jenkins->Script console and type in the following command :

System.setProperty("hudson.model.DirectoryBrowserSupport.CSP", "")

and Press Run.

 If you see the output as 'Result:' below "Result" header then the protection disabled. Re-Run your build and you can see that the new HTML files archived will have the CSS enabled.

Now you can use the HTML publisher plugin to show it.



![](/img-post/开发/NodeJS/Jenkins无法显示Mocha Report.assets/6bef723f6e6849bc8e5e697b0dbe4fac.png)



https://github.com/adamgruber/mochawesome/issues/180

