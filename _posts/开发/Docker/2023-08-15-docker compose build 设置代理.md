---
tags:
    - Docker
---

要在 Docker Compose 中设置代理以在构建镜像时使用，你可以通过以下步骤进行操作：

1. 在 `docker-compose.yml` 文件中找到要设置代理的服务。

2. 在该服务的 `build` 部分添加 `build_args` 设置。`build_args` 定义了构建过程中传递给构建上下文的参数。

3. 在 `build_args` 中添加一个键值对，其中键是你自定义的参数名称，值是代理的配置。

   ```
   yamlCopy Codeservices:
     your_service_name:
       build:
         context: .
         args:
           http_proxy: http://proxy.example.com:8080
           https_proxy: http://proxy.example.com:8080
   ```

   上述示例中，我们为 `your_service_name` 服务设置了 HTTP 和 HTTPS 代理。

4. 根据需要，可以添加其他构建参数，如 `no_proxy` 参数指定不使用代理的主机列表。

   ```
   yamlCopy Codeservices:
     your_service_name:
       build:
         context: .
         args:
           http_proxy: http://proxy.example.com:8080
           https_proxy: http://proxy.example.com:8080
           no_proxy: localhost,127.0.0.1
   ```

5. 保存 `docker-compose.yml` 文件并重新构建你的容器：

   ```
   shellCopy Codedocker-compose build
   ```

在构建过程中，Docker Compose 将传递给构建上下文的参数，并将它们用于相应的服务构建过程中的环境变量或其他配置。