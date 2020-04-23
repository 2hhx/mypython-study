# 红薯小说(js注入)

```
script='''
var span_list = document.getElementsByTagName("span")
for (var i=0;i<span_list.length;i++){
    var content = window.getComputedStyle(
        span_list[i], ':before'
    ).getPropertyValue('content');
    span_list[i].innerText = content.replace('"',"").replace('"',"");
}
''' 
```

## 分析

```
# @Author : SkyOceanchen # @File : hongshu.py
# 红薯小说的文章详情被禁止检测
# 在边框禁止不严谨,可以在边框点击
# 开发者工具
# 在新打开的页面,调出开发者模式,复制链接进行粘贴
# https://www.hongshu.com/content/86235/145336-12522370.html
# url = 'https://www.hongshu.com/content/86235/145336-12522370.html'
# from requests_html import HTMLSession
#
# session = HTMLSession()
# headers = {
#     'User-Agent': 'Mozilla/5.0 (Windows NT 10.0; Win64; x64) AppleWebKit/537.36 (KHTML, like Gecko) Chrome/78.0.3904.70 Safari/537.36'
# }
# r = session.get(url=url)
# print(r.html.text)

# 查询解密算法
# decrypt

```

## 代码

```
from requests_html import HTMLSession, HTML
import execjs
url = 'https://www.hongshu.com/content/74539/122332-11515032.html'
html = HTML(html=url)
search_obj = html.search('https://www.hongshu.com/content/{bid}/{jid}-{cid}.html')

# print(search_obj)
bid = search_obj['bid']
jid = search_obj['jid']
cid = search_obj['cid']
print(bid,jid,cid)
session = HTMLSession()
# headless=False
book_api = 'https://www.hongshu.com/bookajax.do'
"""
method: getchptkey
bid: 86235
cid: 12522370
"""
data = {
    'method': 'getchptkey',
    'bid': bid,
    'cid': cid,
}
r = session.post(url=book_api,data=data)
print(r)#<Response [200]>
print(r.json())#{'code': 119, 'msg': '获取章节内容成功', 'key': '34820512'}
key = r.json().get('key')
print(key)
# 二次请求,渲染数据
"""
method: getchpcontent
bid: 86235
jid: 145336
cid: 12522370
"""
data = {
    'method': 'getchpcontent',
    'bid': bid,
    'jid':jid,
    'cid': cid,
}
r = session.post(url=book_api,data=data)
# print(r.json())
content = r.json().get('content')
# 注意有的other是没有进行加密的
other = r.json().get("other")
# print(content)
# print(other)
js_code = open('hongshu.js','rt',encoding='utf-8').read()
js_obj = execjs.compile(js_code)
content = js_obj.call('decrypt',content,key)
other = js_obj.call('decrypt',other,key)
#  File "D:\python36\lib\subprocess.py", line 1063, in _readerthread
#     buffer.append(fh.read())
# 编码冲突,修改编码,大约在594行pass_fds=(), *, encoding='None', errors=None):
#  encoding='None',将None修改为utf-8
# print(1,content,len(content))
# print(other)# 控制一些隐藏字段的显示，可以建立html进行验证
# 此处控制台打印不出来，原因是控制台可能错误，在cmd中执行
with open('hongshu.html','w',encoding='utf8') as f:
    f.write(content)
#     处理原来不是标签的返回值，变为标签，方便处理
html = HTML(html=content)
# print(html.text)
#
content = html.text
# html.render(reload=False)
# print(other)
# print(content)

# 拼写html，执行other,进行渲染
html='<html><head><meta charset="UTF-8"><script>'+other+'</script></head><body>'+content+'</body></html>'
# 产生html对象
html = HTML(html=html)
# 调用浏览器内核进行渲染
html.render(reload=False)
# html文本
# print(html.html)
# print(html.text)
# print(html.find('body',first=True).text)

script='''
var span_list = document.getElementsByTagName("span")
for (var i=0;i<span_list.length;i++){
    var content = window.getComputedStyle(
        span_list[i], ':before'
    ).getPropertyValue('content');
    span_list[i].innerText = content.replace('"',"").replace('"',"");
}
'''
html.render(reload=False,script=script)
print(html.find('body',first=True).text)
# r = session.get(url=url)
# r.html.render(script=script)
# print(r.html.text)
```

## js

```
function base64decode(str) {
    var c1, c2, c3, c4, base64DecodeChars = new Array(-1,-1,-1,-1,-1,-1,-1,-1,-1,-1,-1,-1,-1,-1,-1,-1,-1,-1,-1,-1,-1,-1,-1,-1,-1,-1,-1,-1,-1,-1,-1,-1,-1,-1,-1,-1,-1,-1,-1,-1,-1,-1,-1,62,-1,-1,-1,63,52,53,54,55,56,57,58,59,60,61,-1,-1,-1,-1,-1,-1,-1,0,1,2,3,4,5,6,7,8,9,10,11,12,13,14,15,16,17,18,19,20,21,22,23,24,25,-1,-1,-1,-1,-1,-1,26,27,28,29,30,31,32,33,34,35,36,37,38,39,40,41,42,43,44,45,46,47,48,49,50,51,-1,-1,-1,-1,-1);
    var i, len, out;
    len = str.length;
    i = 0;
    out = "";
    while (i < len) {
        do {
            c1 = base64DecodeChars[str.charCodeAt(i++) & 0xff];
        } while (i < len && c1 == -1);if (c1 == -1)
            break;
        do {
            c2 = base64DecodeChars[str.charCodeAt(i++) & 0xff];
        } while (i < len && c2 == -1);if (c2 == -1)
            break;
        out += String.fromCharCode((c1 << 2) | ((c2 & 0x30) >> 4));
        do {
            c3 = str.charCodeAt(i++) & 0xff;
            if (c3 == 61)
                return out;
            c3 = base64DecodeChars[c3];
        } while (i < len && c3 == -1);if (c3 == -1)
            break;
        out += String.fromCharCode(((c2 & 0XF) << 4) | ((c3 & 0x3C) >> 2));
        do {
            c4 = str.charCodeAt(i++) & 0xff;
            if (c4 == 61)
                return out;
            c4 = base64DecodeChars[c4];
        } while (i < len && c4 == -1);if (c4 == -1)
            break;
        out += String.fromCharCode(((c3 & 0x03) << 6) | c4);
    }
    return out;
}


function hs_decrypt(str, key) {
    if (str == "") {
        return "";
    }
    var v = str2long(str, false);
    var k = str2long(key, false);
    var n = v.length - 1;
    var z = v[n - 1]
      , y = v[0]
      , delta = 0x9E3779B9;
    var mx, e, q = Math.floor(6 + 52 / (n + 1)), sum = q * delta & 0xffffffff;
    while (sum != 0) {
        e = sum >>> 2 & 3;
        for (var p = n; p > 0; p--) {
            z = v[p - 1];
            mx = (z >>> 5 ^ y << 2) + (y >>> 3 ^ z << 4) ^ (sum ^ y) + (k[p & 3 ^ e] ^ z);
            y = v[p] = v[p] - mx & 0xffffffff;
        }
        z = v[n];
        mx = (z >>> 5 ^ y << 2) + (y >>> 3 ^ z << 4) ^ (sum ^ y) + (k[p & 3 ^ e] ^ z);
        y = v[0] = v[0] - mx & 0xffffffff;
        sum = sum - delta & 0xffffffff;
    }
    return long2str(v, true);
}

function long2str(v, w) {
    var vl = v.length;
    var sl = v[vl - 1] & 0xffffffff;
    for (var i = 0; i < vl; i++) {
        v[i] = String.fromCharCode(v[i] & 0xff, v[i] >>> 8 & 0xff, v[i] >>> 16 & 0xff, v[i] >>> 24 & 0xff);
    }
    if (w) {
        return v.join('').substring(0, sl);
    } else {
        return v.join('');
    }
}
function str2long(s, w) {
    var len = s.length;
    var v = [];
    for (var i = 0; i < len; i += 4) {
        v[i >> 2] = s.charCodeAt(i) | s.charCodeAt(i + 1) << 8 | s.charCodeAt(i + 2) << 16 | s.charCodeAt(i + 3) << 24;
    }
    if (w) {
        v[v.length] = len;
    }
    return v;
}

function utf8to16(str) {
    var out, i, len, c;
    var char2, char3;
    out = "";
    len = str.length;
    i = 0;
    while (i < len) {
        c = str.charCodeAt(i++);
        switch (c >> 4) {
        case 0:
        case 1:
        case 2:
        case 3:
        case 4:
        case 5:
        case 6:
        case 7:
            out += str.charAt(i - 1);
            break;
        case 12:
        case 13:
            char2 = str.charCodeAt(i++);
            out += String.fromCharCode(((c & 0x1F) << 6) | (char2 & 0x3F));
            break;
        case 14:
            char2 = str.charCodeAt(i++);
            char3 = str.charCodeAt(i++);
            out += String.fromCharCode(((c & 0x0F) << 12) | ((char2 & 0x3F) << 6) | ((char3 & 0x3F) << 0));
            break;
        }
    }
    return out;
}

function decrypt(str,key) {
    return utf8to16(hs_decrypt(base64decode(str), key));
}
```

