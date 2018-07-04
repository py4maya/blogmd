---
title: golang使用yaml做配置信息
date: 2018-07-04 13:30:55
tags: [golang,yaml]
categories:
---

* 安装 gopkg.in/yaml.v2

```shell
$ go get gopkg.in/yaml.v2
```

```golang

package main

/****

yaml document content:

enable: true
path: /usr/local
#white: [ hello world, one item, two items ]
white:
- 1
- 2
- 3
- 4
item:
    name: aries
    port: 22
black:
-
  name: 10.210.12.17
  port: 6379
-
  name: 10.210.12.18
  port: 3306

nodes:
- { name: 10.210.12.17 , port: 3306}
- { name: 10.210.12.18 , port: 3306}
- { name: 10.211.221.3, port: 3306}

*****/

import (
	"gopkg.in/yaml.v2"
	"io/ioutil"
	"log"
)

type ConfigType struct {
	Enabled bool                `yaml:"enabled"`
	Path    string              `yaml:"path"`
	White   []string            `yaml:white`
	Black   []map[string]string `yaml:black`
	Item    map[string]string   `yaml:item`
	Nodes   []map[string]string `yaml:nodes`
}

var (
	Config ConfigType
)

func init() {
	yamlFile, err := ioutil.ReadFile("conf.yaml")
	if err != nil {
		log.Printf("yamlFile.Get err   #%v ", err)
	}
	err = yaml.Unmarshal(yamlFile, &Config)
	if err != nil {
		log.Fatalf("Unmarshal: %v", err)
	}

}

```

[YAML语法预览](http://nodeca.github.io/js-yaml/)
