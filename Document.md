### 全局变量:

```
// 上一级的结果
result

// 请求信息,在请求完成后可获取到
params.request

// 响应信息,在请求完成后可获取到
params.response

// 规则基本信息,如host,httpHeaders
config

// params是一个解析基本变量,主要保存上级解析信息和一些变量

params.keyWord
params.pageIndex

// 更多键过滤器
params.filters

// 发现页tab索引;
params.tabIndex

```

### 规则内置CryptoJS,可直接使用

### tools方法:
```
// css选择器
await tools.cssSelector(html,css)

// xpath选择器
await tools.xpathSelector(html,xpath)

// RSA加解密
await tools.rsaEncrypt(string,publicKey);
await tools.rsaDecrypt(string,privateKey);

// RSA加解密(私钥加密-公钥解密)
await tools.rsaEncryptWithPrivate(string,privateKey);
await tools.rsaDecryptWithPublic(string,publicKey);

// 启动一个本地http服务器,content可传递自定义内容,成功将返回一个可访问的本地url
await tools.httpServer(content,suffix);

// 发送http请求
await tools.httpRequest()
await tools.http.post(url,body,headers)
await tools.http.get(url,headers)

// CryptoJS 封装方法
md5Encode: (str) => CryptoJS.MD5(str).toString().toLowerCase(),
base64Encode: (str) => CryptoJS.enc.Base64.stringify(CryptoJS.enc.Utf8.parse(str)),
base64Decode: (str) => CryptoJS.enc.Base64.parse(str).toString(CryptoJS.enc.Utf8),
sha1Encode: (str) => CryptoJS.SHA1(str).toString(),
sha224Encode: (str) => CryptoJS.SHA224(str).toString(),
sha256Encode: (str) => CryptoJS.SHA256(str).toString(),
sha348Encode: (str) => CryptoJS.SHA384(str).toString(),
sha512Encode: (str) => CryptoJS.SHA512(str).toString(),
ripemd160Encode: (str) => CryptoJS.RIPEMD160(str).toString(),
// 调用方法
let res = tools.md5Encode('MD5');
console.log(res);

// 存取本地缓存
await tools.setCache(strKey, obj)
await tools.getCache(strKey)

// 1.0.6≥版本生效
console.log([...])
console.warn([...])
console.error([...])

```


### 视频嗅探
支持开启WebView嗅探视频地址
```
{
    "url":"视频播放地址",
    "webview":true,                      // 非空则开启webview访问
    "sourceRegex":"(.m3u8|.mp4)",        // 嗅探正则表达式
    "notSourceRegex":"(url=|m3u8.js)",   // 过滤嗅探到url
    "webviewJs":"当webview执行完毕后需要运行的脚本,必须提供返回值",
    "webviewJsDelay":5000,               // 脚本执行延迟,单位为毫妙,默认为1秒
}
```


### 请求信息
支持单行

```
/vod-so/$keyWord----------$pageIndex---.html
```

请求对象

```
{
    "url":"https://www.baidu.com/",                             // 请求URL
    "forbidredirect":true,                                      // 禁止重定向
    "headers":{"Referer":"https://www.baidu.com/"},             // 请求头
    "h2":true,                                                  // 是否用http2
    "body":"a=1",                                               // post数据
    "method":"POST",                                            // 请求模式,默认GET
    "cachetime":"3600",                                         // 请求缓存时间,单位为秒
    "webView":true,                                             // 非空就表示启用webview访问完整网页
    "sourceRegex":".m3u8|.mp4",                                 // 正则表达式,启用webview后需要嗅探匹配资源链接
    "notSourceRegex":"url=",                                    // 正则表达式,与[sourceRegex]一起使用,匹配非资源链接
    "webviewJs":"let result = '你好';result",                    // webView运行完成时需要执行的js代码,必须提供返回值
    "webviewJsDelay":1000,                                      // webviewJs执行延迟,单位为毫秒
}
```

禁止重定向
```
{
    "url": "/modules/article/search.php?searchkey=$keyWord&searchtype=articlename&page=$pageIndex",
    "forbidredirect": true
}
```


使用http2请求
```{
    "url": "/modules/article/search.php?searchkey=$keyWord&searchtype=articlename&page=$pageIndex",
    "h2": true
}
```


gbk
```
{
    "url": "/modules/article/search.php?searchkey=$keyWord&searchtype=articlename&page=$pageIndex",
    "encoding": "gbk"
}
```
get-headers
```
{
    "url": "/modules/article/search.php?searchkey=$keyWord&searchtype=articlename&page=$pageIndex",
    "headers":{
        "Accept-Language": "zh-CN,zh;q=0.9,en;q=0.8,en-GB;q=0.7,en-US;q=0.6",
        "User-Agent": "Mozilla/5.0 (Macintosh; Intel Mac OS X 10_15_7) AppleWebKit/537.36 (KHTML, like Gecko) Chrome/116.0.0.0 Safari/537.36"
    }
}
```
post-headers
```
{
    "url": "/modules/article/search.php",
    "method": "POST",
    "body": "searchkey=$keyWord&searchtype=articlename",
    "headers": {
        "Content-Type": "application/x-www-form-urlencoded",
        "Accept-Language": "zh-CN,zh;q=0.9,en;q=0.8,en-GB;q=0.7,en-US;q=0.6",
        "User-Agent": "Mozilla/5.0 (Macintosh; Intel Mac OS X 10_15_7) AppleWebKit/537.36 (KHTML, like Gecko) Chrome/116.0.0.0 Safari/537.36"
    }
}
```

# 封面解密JS
    需要编写JS代码,注意!!!不需要以@js:开头
    其中[coverParam]变量提供图片url与图片数据
    返回值结尾必须是json字符串,看下边例子



```
// coverParam.type      ;类型,为string数据 'list'|'detail'|'content'
                        ;'list'     表示搜索列表或发现列表的封面
                        ;'detail'   表示详情页的封面
                        ;'content'  正文的图片
// coverParam.bytes     ;图片数据为List<int>
// coverParam.url       ;图片加载Url


let a = "my2ecret782ecret";
let r = new Uint8Array(coverParam.bytes);
let s = r, i = CryptoJS.enc.Utf8.parse(a), l = CryptoJS.lib.WordArray.create(s), d, f = o(CryptoJS.AES.decrypt({
    ciphertext: l
}, i, {
    iv: i,
    padding: CryptoJS.pad.Pkcs7
}));

return JSON.stringify({ bytes: Array.from(f) });

```
