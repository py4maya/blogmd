---
title: jupyter 配置文件
date: 2019-06-26 11:25:12
tags: [jupyter]
categories: python
---

* 生成配置:
```shell
jupyter notebook --generate-config
```

* 修改配置:
```python
c.NotebookApp.ip = '10.41.13.133'
c.NotebookApp.notebook_dir = '.notebook/'
c.NotebookApp.open_browser = False
### 密码生成 : from notebook.auth import passwd ; passwd()
c.NotebookApp.password = 'sha1:bde2e6d43d20:7ea9c0f5fd382a294cc3fcc125cd7af4684fe434'
c.NotebookApp.port = 8888
```

* 启动server:

```shell
jupyter notebook
```

* nginx 做反向代理:

```shell
 location /playground {

     proxy_set_header Host $host;
     proxy_set_header X-Real-IP $remote_addr;
     proxy_set_header X-Forwarded-For $proxy_add_x_forwarded_for;
     proxy_set_header REMOTE-HOST $remote_addr;

     proxy_http_version 1.1;
     proxy_set_header Upgrade $http_upgrade;
     proxy_set_header Connection "upgrade";
     proxy_read_timeout 120s;
     proxy_next_upstream error;

     proxy_pass http://127.0.0.1:8888;
 }

 ### 需要设置 jupyter : 
 ### c.NotebookApp.allow_remote_access = True
 ### c.NotebookApp.base_url = '/playground/'
```


