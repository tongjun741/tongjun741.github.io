---
tags:
    - 其它
    - Electron
---

```
cd projectDir
tyarn add electron-chromedriver  selenium-webdriver mocha mocha-appveyor-reporter -D
# package.json的scripts增加： "dist": "electron-builder"
npm run dist
%@taskkill /im chromedriver.exe /f%
%@start /b .\node_modules\.bin\chromedriver%
node_modules\.bin\mocha --reporter mocha-appveyor-reporter b.js
```



```
curl -H "Authorization: Bearer v2.o02ky601udd3k560t6qi" -H "Content-Type: application/json" http://192.168.5.86/api/account/AppVeyor/projects
curl -X POST -H "Authorization: Bearer v2.o02ky601udd3k560t6qi" -H "Content-Type: application/json" http://192.168.5.86/api/account/AppVeyor/builds  -d '{    "accountName": "AppVeyor",    "projectSlug": "ztree-v3",    "branch": "master"}'
curl -H "Authorization: Bearer v2.o02ky601udd3k560t6qi" -H "Content-Type: application/json" http://192.168.5.86/api/projects/AppVeyor/ztree-v3
curl -H "Authorization: Bearer v2.o02ky601udd3k560t6qi" -H "Content-Type: application/json" http://192.168.5.86/api/projects/AppVeyor/ztree-v3/build/1.0.8
```



```
const webdriver = require('selenium-webdriver')

async function example() {
  const driver = new webdriver.Builder()
    // "9515" 是ChromeDriver使用的端口
    .usingServer('http://localhost:9515')
    .withCapabilities({
      'goog:chromeOptions': {
        // 这里填您的Electron二进制文件路径。
        binary: 'C:\\Users\\appveyor\\projects\\\\electron-quick-start\\dist\\win-unpacked\\electron-quick-start.exe'
      }
    })
    .forBrowser('chrome') // 注意: 使用 .forBrowser('electron') for selenium-webdriver <= 3.6.0
    .build()
  try {
    await driver.get('https://aaa.bbb.com');
    // await driver.findElement(By.name('q')).sendKeys('webdriver', Key.RETURN);
    await driver.wait(() => {
      console.log('xx');
      return driver.getTitle().then((title) => {
        console.log("title", title);
        return title === 'xxx'
      })
    }, 1000)
    console.log('done');
  }catch(e){
    console.error(e);
  } finally {
    console.log('quit');
    await driver.quit();
  }
}

example();

```



https://www.electronjs.org/docs/tutorial/using-selenium-and-webdriver#%E9%80%9A%E8%BF%87-webdriverjs-%E9%85%8D%E7%BD%AE