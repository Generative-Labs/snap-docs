---
sidebar_position: 1
---

# Intro

mq-web3 snap provides more possibilities for building web3 social dapps

You can familiarize yourself with mq-web3 snap based on the following example

Before starting, you need to install the [flask plugin](https://chrome.google.com/webstore/detail/metamask-flask-developmen/ljfoeinjpaedjfecbmggjgodbgkmjkjk?hl=zh-CN) on your browser. After the plugin is installed, execute the following code in your dapp to install the mq-web3 snap

## Installation

```javascript
const connect = async function () {
  await window.ethereum.request({
    method: "wallet_enable",
    params: [
      {
        wallet_snap: {
          ["npm:web3-mq-snaps"]: {
            version: "^1.0.6",
          },
        },
      },
    ],
  });
};
connect();
```

## Methods

| name              | type     | Parameters Description                                                                                     | response                                                                                                  |
| ----------------- | -------- |------------------------------------------------------------------------------------------------------------| --------------------------------------------------------------------------------------------------------- |
| creatRoom         | function | 1.group_name?: string 2. avatar_url ?: string                                                              | Object: Web3-MQ Client                                                                                    |
| queryChannelList  | function | 1. options: [PageParams](https://docs.web3mq.com/docs/Web3MQ-SDK/JS-SDK/types/#pageparams)                 | Promise: [activechanneltype](https://docs.web3mq.com/docs/Web3MQ-SDK/JS-SDK/types/#activechanneltype)[]   |
| getTargetUserId   | function | 1. address: string                                                                                         | Promise: [searchusersresponse](https://docs.web3mq.com/docs/Web3MQ-SDK/JS-SDK/types/#searchusersresponse) |
| sendMessage       | function | 1. msg: string 2. topic : string                                                                           | Promise: void                                                                                             |
| getMessageList    | function | 1. options: [PageParams](https://docs.web3mq.com/docs/Web3MQ-SDK/JS-SDK/types/#pageparams) 2.topic: string | Promise: [messagelistitem](https://docs.web3mq.com/docs/Web3MQ-SDK/JS-SDK/types/#messagelistitem)[]       |
| sendNotifyMessage | function | 1. message: string                                                                                         | Promise: void                                                                                             |

## Usage

Execute command Let snap execute logic in ESE and return the return value to you

```javascript
const sendMessage = async function () {
  const msg = "Hello snaps";
  const topic = "user:daeb1886610c47430790cf1f20cba93936d418adba5857e678210c40"; // web3mq's userid
  try {
    const res = await ethereum.request({
      method: "wallet_invokeSnap",
      params: [
        "npm:web3-mq-snaps",
        {
          method: "sendMessage",
          params: { msg, topic },
        },
      ],
    });
    console.log("res", res);
    if (!res) return;
    return res;
  } catch (err) {
    console.error(err);
    alert(`Problem happened: ${err.message}` || err);
  }
};
sendMessage();
```
