# Nodejs-BiliBili-Watcher
[![FOSSA Status](https://app.fossa.com/api/projects/git%2Bgithub.com%2FNekokeCore%2FNodejs-BiliBili-Watcher.svg?type=shield)](https://app.fossa.com/projects/git%2Bgithub.com%2FNekokeCore%2FNodejs-BiliBili-Watcher?ref=badge_shield)
[![Codacy Badge](https://app.codacy.com/project/badge/Grade/a57845ed8b0b491ba08de7213124d076)](https://www.codacy.com/gh/NekokeCore/Nodejs-BiliBili-Watcher/dashboard?utm_source=github.com&amp;utm_medium=referral&amp;utm_content=NekokeCore/Nodejs-BiliBili-Watcher&amp;utm_campaign=Badge_Grade)
![Github](https://img.shields.io/github/last-commit/NekokeCore/Nodejs-BiliBili-Watcher)
![Github](https://img.shields.io/github/issues/NekokeCore/Nodejs-BiliBili-Watcher)
[![](https://img.shields.io/badge/blog-@NekokeCore-blue.svg)](https://www.emtips.com)


## 简介
  一个基于Node.js的B站UP主数据统计系统

## 特点
-  轻量化
-  可自定义程度高
-  网页统计
-  ~~可并发~~ (在做了在做了)

## 依赖
1.  Node.js
2.  jq

## 操作步骤&搭建步骤
1.  安装依赖（以树莓派为例子，以apt为包管理器的Linux系统均可）
```Bash
sudo apt-get update
sudo apt-get install -y nodejs
sudo apt-get install -y jq
sudo apt-get install -y git
sudo apt-get install -y nano
```
2.  Clone本项目
```Bash
mkdir workspace
cd workspace
git clone https://github.com/NekokeCore/Nodejs-BiliBili-Watcher
```
3.  初始化项目
```Bash
cd Nodejs-BiliBili-Watcher/Watcher/Kanasho
chmod -R 755 *
npm install md5 --save
npm install express --save
npm install body-parser --save
```
4.  修改相关配置（本项目以叶笙为例子）
```Bash
nano Kanasho.sh
# 修改 SESSDATA=XXXXXXXXXXXXXXXXXX 为你的账户 Cookie
# 方法为: 
# 在浏览器中（推荐Chrome或者Edge或者FireFox），在你的B站空间主页按F12呼出开发者控制台
# 找到应用程序一项，找到Cookie，可能有两个随便哪一个都可以
# 找到SESSDATA复制好后面的值，在Kanasho.sh对应的位置粘贴
```
5.  启动服务
```Bash
nodejs main.js
#当出现文字提示时说明启动成功
```

## 自定义设置
修改json.sh文件
```Bash
nano json.sh
```
文件内容如下

```Bash
#! /bin/bash
time=$(date "+%c")

JQ_EXEC=`which jq`

FILE_PATH="$1"

fans=$(cat $FILE_PATH | ${JQ_EXEC} ."$2" | sed 's/\"//g')
name=$(cat $FILE_PATH | ${JQ_EXEC} .data.card.name | sed 's/\"//g')

echo "当前时间:$time"
echo "昵称:$name"
echo "粉丝数:$fans"

#可以看到格式是 <变量名>=$(cat $FILE_PATH | ${JQ_EXEC} .第一级.第二级.项目 | sed 's/\"//g')

#咱们看看Json的格式吧

{
    "code":0,
    "message":"0",
    "ttl":1,
    "data":{
        "card":{
            "mid":"21370617",
            "name":"叶笙Kanasho",
            "approve":false,
            "sex":"男",
            "rank":"10000",
            "DisplayRank":"0",
            "regtime":0,
            "spacesta":0,
            "birthday":"",
            "place":"",
            "description":"",
            "article":0,
            "attentions":[],
            "fans":2853,
            "friend":884,
            "attention":884,
            "sign":"来自尼姆诺的史莱姆狼狼 Q群:1097348522 联动等请b站私聊",
            "level_info":{"current_level":5,"current_min":0,"current_exp":0,"next_exp":0},
            "following":false,
            "archive_count":56,
            "article_count":0,
            "follower":2853
        }
    }
}

# 可以看到有好多分级 比如我想让它输出他的关注数 你就可以在json.sh里面新建项

attention=$(cat $FILE_PATH | ${JQ_EXEC} .data.card.attention | sed 's/\"//g')
echo "关注数:$attention"
```
## 感谢

社会易姐QAQ的[B站API](https://github.com/SocialSisterYi/bilibili-API-collect)

[Tony32](https://github.com/TTTTTony32)