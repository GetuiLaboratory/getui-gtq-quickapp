# 个推快应用推送集成文档
通过集成个推快应用推送SDK，用户服务端可以直接调用个推服务端的接口来发送推送消息，由于个推推送服务支持各大厂商推送，因此用户不用再费力地集成各大手机厂商的服务端接口，集成个推一家即可。

## 支持厂商
最新支持情况见：https://doc.quickapp.cn/features/service/push.html

| 厂商       |  支持   | 备注                                                         |
| :--------- | :-----: | :----------------------------------------------------------- |
| 小米       | **YES** | [小米消息推送服务](https://dev.mi.com/console/appservice/push.html) |
| 中兴       |  *no*   | -                                                            |
| 华为       | `1020+` | [华为开发者联盟](https://developer.huawei.com/consumer/cn/console#/serviceCards/AppService) |
| 金立       | `1010+` | [金立快应用开发者中心](http://devquickapp.gionee.com/)       |
| 联想       |  *no*   | -                                                            |
| 魅族       | `1010+` | [魅族集成推送服务](https://open.flyme.cn/openNew/intergrate.html) |
| 努比亚     |  *no*   | -                                                            |
| OPPO       | **YES** | [OPPO 消息推送服务](https://push.oppo.com/)                  |
| vivo       |  *no*   | -                                                            |
| 一加       |    -    | -                                                            |
| **预览版** |  *no*   | 预览版不提供推送接口                                         |



## 接口声明

```json
"features": [
    {
      "name": "system.network"
    },
    {
      "name": "system.device"
    },
    {
      "name": "system.fetch"
    },
    {
      "name": "system.storage"
    },
    {
      "name": "system.app"
    },
    {
      "name": "system.cipher"
    },
    {
      "name": "service.push"
    }
  ],
```



## 初始化

```js
import GtPush from 'GTQ-1.0.0-MIN'

//打开调试模式
GtPush.setDebugMode(true)

//初始化推送SDK，生成cid，服务端可通过此cid推送消息到该设备
GtPush.init({

    appid: "123123",

    success: (data) => {

        console.log(`subscribe success, cid: ${data.cid}`)

    },

    fail: (code, msg) => {

        console.log(`subscribe fail, code: ${code}, msg: ${msg}`)

    }

})

//订阅透传消息
GtPush.subscribe({

    callback: (msgId, data) => {

        console.log(`receive msg, msgId: ${msgId}, data: ${data}`)

    }

})
        
```



## API列表

```js
declare namespace GtPush {
    /**
     * 打开或关闭调试模式
     * @param debugMode 调试模块开启或关闭
     */
    function setDebugMode(debugMode: boolean): void;
    
    /**
     * 初始化推送SDK，生成cid，服务端可通过此cid推送消息到该设备
     *（一般可在应用初始化的地方进行调用。比如在app的onCreate方法中调用。）
     * @param obj
     */
    function init(obj: {
        /**
         * 个推官网发放的appid
         */
        appid: string;
        /**
         * 订阅成功回调
         */
        success?: (data: {
            /**
             * 返回的 clientid，可用于针对某个用户发送消息
             */
            cid: string;
        }) => void;
        /**
         * 订阅失败回调
         */
        fail?: (
        /**
         * 错误码
         */
        code: number, 
        /**
         * 错误信息
         */
        msg: string) => void;
        /**
         * 执行结束后的回调
         */
        complete?: () => void;
    }): Promise<void>;
    
    /**
    * 订阅push，后续可以收到push消息的事件回调（透传消息的payload内容可在此回调中收到）
    * @param obj
    */
    function subscribe(obj: {
        callback: (data: {
            msgId: string;
            data: string;
        }) => void;
    }): void;
    
    /**
    * 取消订阅push，将移除subscribe添加的push事件回调
    */
    function unsubscribe(): void;
}
export default GtPush;

```

