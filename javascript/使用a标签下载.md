### 利用html download属性

[codepen blob download](<https://codepen.io/nico1988/pen/BayWdzo>)



1、使用window.location跳转

```javascript
    var a = document.createElement('a');
      //需要下载的数据内容,我这里放的就是BLOB，如果你有下载链接就不需要了
    var url = window.URL.createObjectURL(content);
    var filename = 'XXX.zip';
    a.href = url;
    a.download = filename;
    a.click();
    window.URL.revokeObjectURL(url);

```
2、后台返回文件的时候设置文件http头Content-Disposition

```nodejs

```

3、使用iframe或者a标签



```javascript

```

显示进度条

```
var xhr = new XMLHttpRequest();
xhr.open('GET', url);
xhr.onprogress = function (event) {
    console.log(Math.round(event.loaded / event.total * 100) + "%");
};
xhr.responseType = 'arraybuffer';
xhr.addEventListener('readystatechange', function (event) {
    if (xhr.status === 200 &amp;&amp; xhr.readyState === 4) {
        // 获取响应头主要获取附件名称
        var contentDisposition = xhr.getResponseHeader('content-disposition');
        // 获取类型类型和编码  
        var contentType = xhr.getResponseHeader('content-type');
        // 构造blob对象,具体看头部提供的链接地址
        var blob = new Blob([xhr.response], {
            type: contentType
        });
        var url = window.URL.createObjectURL(blob);
        // 获取文件夹名
        var regex = /filename=[^;]*/;
        var matchs = contentDisposition.match(regex);
        if (matchs) {
            filename = decodeURIComponent(matchs[0].split("=")[1]);
        } else {
            filename = +Date.now() + ".doc";
        }
        var a = document.createElement('a');
        a.href = url;
        a.download = filename;
        a.click();
        window.URL.revokeObjectURL(url);
        // dosomething
    }
})
xhr.send();

```



### 参考

[mdn blob](<https://developer.mozilla.org/en-US/docs/Web/API/Blob>)

[为什么视频网站的视频链接地址是blob？](<https://juejin.im/post/5d1ea7a8e51d454fd8057bea>)