---
tags:
    - 其它
    - Git
    - GitHub Actions
---

```

jobs:

  Windows10:
    runs-on: windows-latest
    
    steps:
      - name: 修改分辨率为1920*1080
        run: |
          Set-DisplayResolution -Width 1920 -Height 1080 -Force
```

似乎只能用1920*1080，其它分辨率可能不支持。



https://github.com/actions/runner-images/issues/2935

https://github.com/actions/runner-images/issues/2933

https://github.com/actions/runner-images/issues/8606