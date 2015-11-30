---
layout: post
title: "两种方法抓取落网所有专辑的音乐"
categories: [爬虫]
---

在落网上听音乐是一个非常享受的事情，唯美的封面、文字，瞬间就觉得自己置身于一个没有喧嚣、没有嘈杂的环境里，一切尽在不言中～
最近在看爬虫相关的资料，想想正好用python爬爬落网练练手，说干就干，下面大致说下我爬落网的过程:

## 明确目标

1. 爬下落网所有期的音乐，并按每期的名字建立目录，将该期的音乐存在所属期的目录下
2. 程序能自动下载当前的最新一期
3. 每首音乐需正确命名，因为落网的音乐链接使用编号命名的，得改用正确的歌名命名

先来两张图看看效果：

* 目录：
![](http://ww4.sinaimg.cn/large/6120fe13jw1entwctqx52j20wh06pdkg.jpg)

* 文件：
![](http://ww3.sinaimg.cn/large/6120fe13jw1entwf5kdp1j20kb070jv3.jpg)

## 前期准备
先说下我所用的工具：Archlinux + Emacs24.4 + python2.7 + firefox

Q：该用什么第三方库？
> python关于爬虫的第三方库何其多，这里我提供两套方案：
>
> 1. [Requests](http://requests.readthedocs.org/zh_CN/latest/) 加载网页 + [BeautifulSoup](http://www.crummy.com/software/BeautifulSoup/bs4/doc/index.zh.html) 解析网页
> 2. [Scrapy](http://scrapy.org)，`xpath`很好用

## 抓取思路
两套方案的思路都是一样的：首先我得知道最新一期的编号，如果比我已经下载的期数大，则从此期数开始遍历一直到已经爬过的期数，进入每期单独的页面，得到每期的名字用于建立目录，每期的歌单有多少歌以及歌名用于下载和重命名。代码这块我就只讲`Requests`+`BeautifulSoup`:

1. 获取最新一期的编号，用Firebug查看html：
![](http://ww1.sinaimg.cn/large/6120fe13jw1entxgq1gcnj20fc03cq3o.jpg)

    ```python
    numberUrl = 'http://www.luoo.net'
    def getTheMaxNumber():
        # get the html
        numberResponse = requests.get(numberUrl)
        numberResponse.encoding = 'utf-8'
        numberSoup = BeautifulSoup(numberResponse.text, 'lxml')
        # parse the html and get the number
        numberText = (numberSoup.find_all('div', 'vol-item-lg')[0]).text

        maxNumber = int(re.findall(r'\d+', numberText)[0])
        return maxNumber
    ```

2. 进行比较，先读取保存文件目录下的number.txt看上次下载到了哪一期，如果最新期数大，则进入下载音乐的函数，否则返回：

    ```python
    # compare
    if maxNumber > currentNumber:
        for i in range(currentNumber + 1, maxNumber + 1):
            downloadMusic(i)
        # after download the music, should write the right number to number.txt
        numberFile.write(str(maxNumber) + '\t' + time.strftime('%Y-%m-%d %H:%M:%S') + '\n')
        numberFile.close()
    else:
        # do nothing
        print '您已经下载了所有的期数，谢谢！\n'
        return
    ```

3. 建立目录并下载音乐：
先用Firebug查看页面：

![](http://ww4.sinaimg.cn/large/6120fe13jw1entxwx6hi8j20ao01vweq.jpg)

![](http://ww2.sinaimg.cn/large/6120fe13jw1entxzc87ehj20gn01imxk.jpg)

* 得到期数ID

    ```python
    albumId = (albumSoup.find_all('span', 'vol-number rounded')[0]).text
    ```

* 得到期数名字用来建立目录，最好是将名字间的空格去掉，不然后面wget时会出现问题

    ```python
    albumName = (albumSoup.find_all('span', 'vol-title')[0]).text.replace(' ', '_')
    ```

* 得到每首歌的名字用来重命名音乐

    ```python
    musicList = (albumSoup.find_all('a', 'trackname btn-play'))
    ```

* 建立目录：

    ```python
    isExists = os.path.exists(saveDir)
    if not isExists:
        os.makedirs(saveDir)
    ```

* 使用wget下载mp3:

    ```python
    for i in range(1, len(musicList) + 1):
            currentMusicName = musicList[i - 1].text.replace(' ', '_').replace('._', '.').strip('(').strip(')')
            cmd1 = 'wget ' + musicUrl + str(int(albumId)) + '/' + string.zfill(i,2) + '.mp3 ' + '-O ' + saveDir + currentMusicName + '.mp3'
            cmd2 = 'wget ' + musicUrl + str(int(albumId)) + '/' + str(i) + '.mp3 ' + '-O ' + saveDir + currentMusicName + '.mp3'

            if os.system(cmd1.encode('utf-8')) != 0:
                os.system(cmd2.encode('utf-8'))
    ```

## 完整代码
完整代码请移步我的[github](https://github.com/jlovedragon/CrawLuoo/tree/master/requests_bs4)
另一种方案Scrapy的思路一样，[代码](https://github.com/jlovedragon/CrawLuoo/tree/master/scrapy)

## 参考文献
1. [Requests: HTTP for Humans](http://requests.readthedocs.org/zh_CN/latest/)
2. [Beautiful Soup 4.2.0 文档](http://www.crummy.com/software/BeautifulSoup/bs4/doc/index.zh.html)
3. [Scrapy爬虫抓取网站数据](http://chenqx.github.io/2014/11/09/Scrapy-Tutorial-for-BBSSpider/)

