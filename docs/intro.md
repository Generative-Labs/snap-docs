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
            ["npm:mq-web3"]: {
              version: "^1.0.16",
            },
          },
        },
      ],
    });
  };
 connect()
```
## Usage

Execute command Let snap execute logic in ESE and return the return value to you
```javascript
const send = async function (method, payload) {
    try {
      const res = await window.ethereum.request({
        method: "wallet_invokeSnap",
        params: [
          "npm:mq-web3",
          {
            method,//the name of the command
            payload, //Parameters required to execute the command
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
  const initWeb3Mq = async function(){
     let payload = {
      app_key: "vAUJTFXbBZRkEDRE",
      connectUrl: "",
      env: "test",
    }
     let res = await send('web3-mq-init',payload)
     console.log('res',res)
  }
  initWeb3Mq()
```