## 步骤

#### 1. Fork 本专案到自己的 GitHub 账户（用户名以 `example` 为例）

#### 2. 修改专案名称，注意不要包含 `v2ray` 和 `heroku` 两个关键字（修改后的专案名以 `demo` 为例）

#### 3. 修改 `README.md`，将 `Ccnikola/asd` 替换为自己的内容（如 `example/demo`）

#### 4. 点击按钮来部署

[![Deploy](https://www.herokucdn.com/deploy/button.png)](https://dashboard.heroku.com/new?template=https://github.com/Ccnikola1/PHP)

## 对部署时需设定的变量名称做如下说明。

| 变量 | 默认值 | 说明 |
| :--- | :--- | :--- |
| `ID` | `ad806487-2d26-4636-98b6-ab85cc8521f7` | VMess 用户主 ID，用于身份验证，为 UUID 格式。[UUID在线生成器](https://www.uuidgenerator.net "UUID在线生成器") |
| `WSPATH` | `/` | WebSocket 所使用的 HTTP 协议路径 |

## CloudFlare反代

<details> 
<summary>使用Cloudflare的Workers来中转流量，单双日轮换反代代码(推荐)</summary>

```js
const SingleDay = 'appname1.herokuapp.com'
const DoubleDay = 'appname2.herokuapp.com'
addEventListener(
    "fetch",event => {
    
        let nd = new Date();
        if (nd.getDate()%2) {
            host = SingleDay
        } else {
            host = DoubleDay
        }
        
        let url=new URL(event.request.url);
        url.hostname=host;
        let request=new Request(url,event.request);
        event. respondWith(
            fetch(request)
        )
    }
)
```
</details>

<details>
<summary>使用Cloudflare的Workers来中转流量，单账户反代代码</summary>
 
```js
addEventListener(
  "fetch", event => {
    let url = new URL(event.request.url);
    url.host = "appname.herokuapp.com";
    let request = new Request(url, event.request);
    event.respondWith(
      fetch(request)
    )
  }
)
```

</details>
