# 动态生成二维码推送客户端的 DEMO

这是[奇舞特训营](http://t.75team.com) JavaScript 课程关于与服务器通讯部分的例子代码。

因为用了 async/await 需要 Node 7.6.0 或者用 Babel 编译。

可以参考[这篇文章](https://www.h5jun.com/post/manage_node_with_n.html)安装和管理多版本 Node。

## 安装

```bash
npm install
```

## 轮询

```bash
node polling
```

## 长轮询

```bash
node long_polling
```

## WebSocket

```bash
node websocket.js
```

## 访问

http://127.0.0.1:9999

## 客户端

### 轮询

```html
<script src="http://s4.qhres.com/static/6026082b2fdaf405.js"></script>

<a href="###">
  <div id="qrcode"></div>
</a>

<script>
const qrcode = document.getElementById("qrcode");

function poll(){
  const xhr = new XMLHttpRequest(),
      method = "GET",
      url = "http://teach.h5jun.com/qrcode";

  xhr.open(method, url, true);

  xhr.onreadystatechange = function () {
    if(xhr.readyState === XMLHttpRequest.DONE && xhr.status === 200) {
      //console.log(xhr.responseText);
      let data = JSON.parse(xhr.responseText);
      let url = `http://teach.h5jun.com/check/${data.code}`;
      qrcode.innerHTML = '';
      new QRCode(qrcode, url);
      qrcode.parentNode.href = url;
    }
  };

  xhr.send();
}

poll();
setInterval(poll, 1000);
</script>
```

### 长轮询

```html
<script src="http://s4.qhres.com/static/6026082b2fdaf405.js"></script>

<a href="###">
  <div id="qrcode"></div>
</a>

<script>
const qrcode = document.getElementById("qrcode");

function long_poll(code){
  const xhr = new XMLHttpRequest(),
      method = "GET",
      url = `http://teach.h5jun.com/qrcode/${code}`;

  xhr.open(method, url, true);

  xhr.onreadystatechange = function () {
    if(xhr.readyState === XMLHttpRequest.DONE && xhr.status === 200) {
      //console.log(xhr.responseText);
      let data = JSON.parse(xhr.responseText);
      let url = `http://teach.h5jun.com/check/${data.code}`;
      qrcode.innerHTML = '';
      new QRCode(qrcode, url);
      qrcode.parentNode.href = url;
      long_poll(data.code);
    }
  };

  xhr.send();
}

long_poll(0);
</script>
```

### WebSocket

```html
<script src="http://s4.qhres.com/static/6026082b2fdaf405.js"></script>

<a href="###">
  <div id="qrcode"></div>
</a>

<script>
const ws = new WebSocket('ws://teach.h5jun.com');
const qrcode = document.getElementById("qrcode");

ws.addEventListener('open', function (event) {
    ws.send('open');
});

ws.addEventListener('message', function (event) {
    //console.log('Message from server', event.data);
    
    let url = `http://teach.h5jun.com/check/${event.data}`;
    qrcode.innerHTML = '';
    new QRCode(qrcode, url);
    qrcode.parentNode.href = url;
});
</script>
```