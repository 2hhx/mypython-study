# Python下载哔哩哔哩学习视频

## 0 概要

涵盖技术：哔哩哔哩反爬，解析接口，视频，音频分片下载，视频音频合成

## 一 目标地址

哔哩哔哩上Egon大神的python快速入门视频，咱们要全部爬取下来

<https://www.bilibili.com/video/av73342471?p=1>

![image-20191209173423502](https://tva1.sinaimg.cn/large/006tNbRwgy1g9qlsooa4mj31kb0u04k5.jpg)

## 二 安装ffmpeg

### 2.1mac下安装

只要把项目中的该文件配置到环境变量即可

![image-20191209173650193](https://tva1.sinaimg.cn/large/006tNbRwgy1g9qlv7sfenj30bu0acdgn.jpg)

### 2.2 windows下安装

将项目目录下的该文件解压，把bin目录配置到环境变量即可

![image-20191209173731196](https://tva1.sinaimg.cn/large/006tNbRwgy1g9qlvxocsyj30gu0aawfl.jpg)

## 三 破解分析

### 3.1分析出该地址的接口为

    
    
    https://api.bilibili.com/x/player/pagelist?aid=73342471

请求返还结果为：

![image-20191210012438389](https://tva1.sinaimg.cn/large/006tNbRwgy1g9qzkq9gm5j31h20u04e0.jpg)

### 3.2 分析出每个视频地址为：

每一个视频地址，通过p=数字来区分如第2个视频为：<https://www.bilibili.com/video/av73342471?p=2>

我们向每个地址发送请求，拿回页面，内部含有json数据

通过正则解析出想要的数据：

    
    
    playinfo = my_match(res.text, '__playinfo__=(.*?)</script><script>')

数据格式是这样的，里面有分辨率，不同分辨率的视频地址，音频地址

![image-20191210012649423](https://tva1.sinaimg.cn/large/006tNbRwgy1g9qzl1e10oj30sy128jvy.jpg)

### 3.2分片下载视频和音频

    
    
    def download_video(old_video_url,video_url,audio_url,video_name):
        headers.update({"Referer":old_video_url})
        print("开始下载视频：%s"%video_name)
        video_content = requests.get(video_url, headers=headers)
        print('%s视频大小：'%video_name, video_content.headers['content-length'])
        audio_content = requests.get(audio_url, headers=headers)
        print('%s音频大小：'%video_name, audio_content.headers['content-length'])
        # 下载视频开始
        received_video = 0
        with open('%s_video.mp4' % video_name, 'ab') as output:
            while int(video_content.headers['content-length']) > received_video:
                headers['Range'] = 'bytes=' + str(received_video) + '-'
                response = requests.get(video_url, headers=headers)
                output.write(response.content)
                received_video += len(response.content)
        # 下载视频结束
        # 下载音频开始
        audio_content = requests.get(audio_url, headers=headers)
        received_audio = 0
        with open('%s_audio.mp4' % video_name, 'ab') as output:
            while int(audio_content.headers['content-length']) > received_audio:
                # 视频分片下载
                headers['Range'] = 'bytes=' + str(received_audio) + '-'
                response = requests.get(audio_url, headers=headers)
                output.write(response.content)
                received_audio += len(response.content)
        # 下载音频结束
        return video_name

### 3.3 把视频和音频通过ffmpeg合并到一起即可

    
    
    def make_all(result):
        # 视频音频合并
        video_name=result.result()
        import subprocess
        video_final = video_name.replace('video', 'video_final')
        ss = 'ffmpeg -i %s_video.mp4 -i %s_audio.mp4 -c copy %s.mp4' % (video_name, video_name, video_final)
        subprocess.Popen(ss, shell=True)
        #删除视频和音频
        # os.remove('%s_video.mp4'%video_name)
        # os.remove('%s_audio.mp4'%video_name)
        print("视频下载结束：%s" % video_name)

### 3.4 大功告成

video_final文件夹中放着下好的视频，video中放着分开的视频和音频

![image-20191210013246446](https://tva1.sinaimg.cn/large/006tNbRwgy1g9qzmh67wlj31no0lctii.jpg)

## 四 全部代码

    
    
    #导入requests模块，模拟发送请求
    import requests
    #导入线程池模块，创建30个线程同时做
    from concurrent.futures import ThreadPoolExecutor
    #导入ssl模块，处理https请求失败问题
    import ssl
    #导入json
    import json
    #导入re
    import re
    import os
    #定义请求头
    headers="{'Accept': '*/*', 'Accept-Language': 'en-US,en;q=0.5', 'User-Agent': 'Mozilla/5.0 (Macintosh; Intel Mac OS X 10_12_6) AppleWebKit/537.36 (KHTML, like Gecko) Chrome/63.0.3239.84 Safari/537.36'}"
    headers={
        'Accept':'*/*',
        'Accept-Language':'en-US,en;q=0.5',
        'User-Agent':'Mozilla/5.0 (Macintosh; Intel Mac OS X 10_12_6) AppleWebKit/537.36 (KHTML, like Gecko) Chrome/63.0.3239.84 Safari/537.36'
    }
    #定义一个池，大小为30
    pool = ThreadPoolExecutor(30)
    # 生成证书上下文(unverified 就是不验证https证书)
    ssl._create_default_https_context = ssl._create_unverified_context
    res_json=requests.get('https://api.bilibili.com/x/player/pagelist?aid=73342471').json()
    
    
    #正则表达式，根据条件匹配出值
    def my_match(text,pattern):
        match = re.search(pattern, text)
        # print(match.group(1))
        # print()
        return json.loads(match.group(1))
    #下载并合并视频的函数
    def download_video(old_video_url,video_url,audio_url,video_name):
        headers.update({"Referer":old_video_url})
        print("开始下载视频：%s"%video_name)
        video_content = requests.get(video_url, headers=headers)
        print('%s视频大小：'%video_name, video_content.headers['content-length'])
        audio_content = requests.get(audio_url, headers=headers)
        print('%s音频大小：'%video_name, audio_content.headers['content-length'])
        # 下载视频开始
        received_video = 0
        with open('%s_video.mp4' % video_name, 'ab') as output:
            while int(video_content.headers['content-length']) > received_video:
                headers['Range'] = 'bytes=' + str(received_video) + '-'
                response = requests.get(video_url, headers=headers)
                output.write(response.content)
                received_video += len(response.content)
        # 下载视频结束
        # 下载音频开始
        audio_content = requests.get(audio_url, headers=headers)
        received_audio = 0
        with open('%s_audio.mp4' % video_name, 'ab') as output:
            while int(audio_content.headers['content-length']) > received_audio:
                # 视频分片下载
                headers['Range'] = 'bytes=' + str(received_audio) + '-'
                response = requests.get(audio_url, headers=headers)
                output.write(response.content)
                received_audio += len(response.content)
        # 下载音频结束
        return video_name
    
    
    def make_all(result):
        # 视频音频合并
        video_name=result.result()
        import subprocess
        video_final = video_name.replace('video', 'video_final')
        ss = 'ffmpeg -i %s_video.mp4 -i %s_audio.mp4 -c copy %s.mp4' % (video_name, video_name, video_final)
        subprocess.Popen(ss, shell=True)
        #删除视频和音频
        # os.remove('%s_video.mp4'%video_name)
        # os.remove('%s_audio.mp4'%video_name)
        print("视频下载结束：%s" % video_name)
    
    def main_download():
        for i, video_content in enumerate(res_json['data']):
            video_name = ('./video/' + video_content['part']).replace(" ", "-")
            old_video_url = 'https://www.bilibili.com/video/av73342471' + '?p=%d' % (i + 1)
            # print('视频地址为：',old_video_url)
            # print('视频名字为：',video_name)
            # 加载上面拼凑的视频地址
            res = requests.get(old_video_url, headers=headers)
            # 解析出当前页面下所有视频的json
            # initial_state = my_match(res.text, r'__INITIAL_STATE__=(.*?);\(function\(\)')
            # 解析出视频详情的json
            playinfo = my_match(res.text, '__playinfo__=(.*?)</script><script>')
            # 列表套字典，拼凑出视频和音频的字典（这里只下载1080p的视频）
            video_info_list = []
            for i in range(4):
                video_info = {}
                video_info['quality'] = playinfo['data']['accept_description'][i]
                video_info['acc_quality'] = playinfo['data']['accept_quality'][i]
                video_info['video_url'] = playinfo['data']['dash']['video'][i]['baseUrl']
                video_info['audio_url'] = playinfo['data']['dash']['audio'][0]['baseUrl']
                video_info_list.append(video_info)
            # 1080p视频的地址为，分片加载的
            video_url = video_info_list[0]['video_url']
            # 1080p视频的音频为，分片加载的
            audio_url = video_info_list[0]['audio_url']
            # download_cov(old_video_url,video_url,audio_url,video_name)
            pool.submit(download_video, old_video_url, video_url, audio_url, video_name).add_done_callback(make_all)
        pool.shutdown(wait=True)
    
    
    if __name__ == '__main__':
        main_download()

## 五 写在最后

文章涉及you-get和ffmpeg的使用，这里简单一说，后续有时间详细跟大家聊吧

### ffmpeg

FFmpeg是一套可以用来记录、转换数字音频、视频，并能将其转化为流的开源计算机程序

可以视频格式转换，提取音频，合并视频，视频剪切，给视频加logo。。。无所不能

由于各大视频网站视频都是以视频流的形式，分片传输的，如哔哩哔哩，所以需要分片下载后合并视频，哔哩哔哩用的m4a格式，优酷用的u3m8格式（有兴趣的可以自行学习u3m8视频格式，如何合并成mp4）

![image-20191209174508728](https://tva1.sinaimg.cn/large/006tNbRwgy1g9qm3wnha9j323e0kk7hj.jpg)

### u3m8

m3u8格式链接在浏览器上打开，没有插件的情况下你会得到长得跟下面差不多的一个文本列表

![image-20191209174757565](https://tva1.sinaimg.cn/large/006tNbRwgy1g9qm6s12cjj30oq0do0v1.jpg)

以.ts 结尾的那些就是视频连接的实际播放地址，当然你还要拼上前面的前缀。

在浏览器上安装过插件的情况，你可以直接在线预览影片，但是如果你想下载到本地却很麻烦，在浏览器上传好看网络请求你会发现一部60分钟的影片可能被切成了几百上千个片段，每个片段不到10秒，难道我们要下一千个片段，然后用ffmpeg拼起来

### you-get

一个开源的下载分片视频，通过ffmpeg合并的项目（python写的），有兴趣的可以研究一下源码，地址为：<https://github.com/soimort/you-
get/>

所以，我们可以直接使用you-get下载，代码如下(非常简单)

    
    
    import requests
    from you_get.common import any_download
    from concurrent.futures import ThreadPoolExecutor
    
    import ssl
    pool = ThreadPoolExecutor(10)
    # 生成证书上下文(unverified 就是不验证https证书)
    ssl._create_default_https_context = ssl._create_unverified_context
    res_json=requests.get('https://api.bilibili.com/x/player/pagelist?aid=73342471').json()
    
    def download(url):
        any_download(url, output_dir='video', merge='merge')
    
    for i,video_content in enumerate(res_json['data']):
        video_name=video_content['part']
        video_url='https://www.bilibili.com/video/av73342471'+'?p=%d'%(i+1)
        print('视频地址为：',video_url)
        print('视频名字为：',video_name)
        pool.submit(download, video_url)
    
    pool.shutdown(wait=True)

