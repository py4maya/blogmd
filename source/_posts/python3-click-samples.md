---
title: python3 使用click 构建入口程序 
date: 2018-12-03 16:24:03
tags: [click,python3]
categories:
---

```python

import click

@click.command()
@click.option("-a",default="",help="action you want to do! <syslog,cron>")

### 必须包含a 参数，包含click.option 中定义的所有参数
def do_command(a):
    if a == 'syslog':
        print('start syslog info ....')
    else :
        print('./app.py --help')

if __name__ == '__main__':
    do_command()
```
