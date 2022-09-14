---
sidebar_position: 1
---

# QuickStart

**This Quickstart tutorial walks you through the key concepts of mq-web3 snap using a sample react project and successfully send your first "hello world" to your friend!**


## Installation
:::note

Before installing, you need to install the [flask plugin](https://chrome.google.com/webstore/detail/metamask-flask-developmen/ljfoeinjpaedjfecbmggjgodbgkmjkjk?hl=zh-CN) on your browser.

::: 

Install mq-web3 snap by execute the following method in your app
:::note

Before installation, flask will pop up a window for approval permissions, click the approval and installation button to start the installation program

:::

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
```
After the installation is complete you need a method to execute all the instructions uniformly, let's declare such a utility function
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
```
## Initialize Client and Connect to Web3MQ Network
In order to connect to the Web3MQ Network, both users and developers authenticate through wallet signatures, we demonstrate below with an Ethereum wallet via Metamask, but Web3MQ is built to be compatible with wallets across different chains.

### Connect to Web3MQ Network
:::note

While we are committed to building an open and collectively owned public good, our early stage testnet requires an API key in order to connect. This is to control capacity to make sure that each early partner and developer is able to build a great experience on top of Web3MQ. [Apply here](https://web3mq.com/apply).

:::
We use the unified command execution method send  we declared above to connect to our Web3MQ network.Simply execution web3-mq-init command without connectUrl or an empty string returns a url of the best node determined for you, and this url can be stored locally.

```javascript
const initWeb3Mq = async function () {
  // You can save the bestEndpointUrl locally to skip endpoint search next time, which will save time, and
    const bestEndpointUrl = await send("web3-mq-init", {
      app_key: "",// temporary authorization key obtained by applying, will be removed in future testnets and mainnet
      connectUrl:'',
      env: "test",
    });
    console.log('bestEndpointUrl',bestEndpointUrl)
    initWeb3Mq()
```
execution web3-mq-init command  with a specific connectUrl forces the client to connect to that specific node. When bestEndpointUrl is stored, it might be time-saving to connect directly instead of running the search again.

```javascript
const initWeb3Mq = async function () {
    const bestEndpointUrl = await send("web3-mq-init", {
      app_key: "",// Appkey applied from our team
      connectUrl:bestEndpointUrl,// takes in a valid endpoint url as input, when this paramter is given, client will always connect to that specific node.
      env: "test",
    });
     localStorage.setItem("FAST_URL", bestEndpointUrl);
    console.log('bestEndpointUrl',bestEndpointUrl)
    initWeb3Mq()
```
#### API endpoints

During this initial testing phase, we've hosted complete networks of Web3MQ nodes in different regions around the globe. Connect to these endpoints below, to access the Web3MQ Testnet.

- https://testnet-us-west-1-1.web3mq.com
- https://testnet-us-west-1-2.web3mq.com
- https://testnet-ap-jp-1.web3mq.com
- https://testnet-ap-jp-2.web3mq.com
- https://testnet-ap-singapore-1.web3mq.com
- https://testnet-ap-singapore-2.web3mq.com


### Sign with wallet to register user and obtain message encryption keys

For any first-time user of Web3MQ's network, you'll need to register on Web3MQ's network. mq-web3 snap takes care of the key generation process and subsequent wallet signing process. execution web3-mq-register command to get keys

#### Code

:::note

 You must ensure that execution web3-mq-init command and make sure it is complete before running this
:::

```ts
const register =async function(){
  let result = await send("web3-mq-register", {
      signContentURI: "https://www.web3mq.com",
    });
    console.log("result", result);
    const { PrivateKey, PublicKey } = result;
    localStorage.setItem("PRIVATE_KEY", PrivateKey);
    localStorage.setItem("PUBLICKEY", PublicKey);
}
```
### Get Client Instance

#### Code

```ts
const keys = { PrivateKey, PublicKey };

const generateInstance = async function () {
    return await send("getInstance", { keys });
  };
  generateInstance()
```
## Create room

After initializing the client and registering your user, the next step is to connect to a room

#### Code

```ts
const createRoom = async function () {
    await send("creatRoom", {});
  };
  createRoom()
```

```tsx
<button
  onClick={createRoom}>
  createGroup
</button>
```
## Send message

#### Code

```ts
const sendMessage = async function (payload) {
    return await send("handleSendMessage", { ...payload });
  };
  sendMessage({
    text:'Hello World'
  })
```

```tsx
<button
  onClick={() => {
  sendMessage({
    text:'Hello World'
  });
}}>
  sendMessage
</button>
```
## Full Example

```jsx
import React, { useEffect, useMemo, useState } from "react";

function App() {
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
  const send = async function (method, payloadcus) {
    console.log("method", method);
    let payload = {
      ...payloadcus,
    };
    try {
      const res = await window.ethereum.request({
        method: "wallet_invokeSnap",
        params: [
          "npm:mq-web3",
          {
            method, //the name of the command
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

  const init = async () => {
    await connect();
  };
  const hasKeys = useMemo(() => {
    const PrivateKey = localStorage.getItem("PRIVATE_KEY") || "";
    const PublicKey = localStorage.getItem("PUBLICKEY") || "";
    if (PrivateKey && PublicKey) {
      return { PrivateKey, PublicKey };
    }
    return null;
  }, []);
  const [fastestUrl, setFastUrl] = useState(null);
  const [keys, setKeys] = useState(hasKeys);
  const signMetaMask = async function () {
    await init();
    const fastUrl = await send("web3-mq-init", {
      app_key: "vAUJTFXbBZRkEDRE",
      connectUrl: "",
      env: "test",
    });
    console.log("fast", fastUrl);
    let result = await send("web3-mq-register", {
      signContentURI: "https://www.web3mq.com",
    });
    console.log("result", result);
    const { PrivateKey, PublicKey } = result;
    localStorage.setItem("FAST_URL", fastUrl);
    localStorage.setItem("PRIVATE_KEY", PrivateKey);
    localStorage.setItem("PUBLICKEY", PublicKey);
    setKeys({ PrivateKey, PublicKey });
    setFastUrl(fastUrl);
    console.log("keys", PrivateKey, PublicKey);
  };
  if (!keys || !fastestUrl) {
    return (
      <React.Fragment>
        {/* <button onClick={init}>connect</button> */}
        <div>
          <button onClick={signMetaMask}>signMetaMask</button>
        </div>
      </React.Fragment>
    );
  }

  return (
    <div>
      <Child send={send} keys={keys}></Child>
    </div>
  );
}
const Child = ({ ...props }) => {
  const { send, keys } = props;
  const [list, setList] = useState(null);
  const [activeChannel, setCurrentActiveChannel] = useState(null);
  const [text, setText] = useState("");
  const [messageList, setMessageList] = useState([]);
  const generateInstance = async function () {
    return await send("getInstance", { keys });
  };
  const addInstanceListeners = async function () {
    return await send("addInstanceListeners", {});
  };
  const queryChannelList = async function () {
    return await send("queryChannelList", {});
  };
  const getChannelList = async function () {
    return await send("getChannelList", {});
  };
  const setActiveChannel = async function (payload) {
    return await send("setActiveChannel", { ...payload });
  };
  const getActiveChannel = async function () {
    return await send("getActiveChannel", {});
  };
  const getClientMessageList = async function () {
    return await send("getClientMessageList", {});
  };
  const sendMessage = async function (payload) {
    return await send("handleSendMessage", { ...payload });
  };
  const creatClient = async function () {
    await generateInstance();
    await addInstanceListeners();
    await queryChannelList();
    let list = await getChannelList();
    setList(list);
  };
  useEffect(() => {
    creatClient();
  }, [keys]);
  const intervalFuction = async function () {
    let activeChannel = await getActiveChannel();
    if (activeChannel) {
      let msgList = await getClientMessageList();
      setMessageList(msgList);
    }
  };
  useEffect(() => {
    let timer = setInterval(() => {
      intervalFuction();
    }, 2000);
    return () => {
      clearInterval(timer);
    };
  }, []);
  const handleChangeActive = async function (channel) {
    await setActiveChannel({ channel });
    let activeChannel = await getActiveChannel();
    if (activeChannel) {
      let msgList = await getClientMessageList();
      setMessageList(msgList);
      setCurrentActiveChannel(activeChannel);
    }
  };
  const handleSendMessage = async function () {
    setText("");
    await sendMessage(text);
  };
  const createRoom = async function () {
    await send("creatRoom", {});
    // await queryChannelList();
    let list = await getChannelList();
    setList(list);
  };
  const List = () => {
    if (!list) {
      return null;
    }
    return (
      <ul>
        {list.map((item, idx) => {
          return (
            <li
              style={{ cursor: "pointer" }}
              key={item.topic}
              onClick={() => handleChangeActive(item)}
            >
              {idx}-{item.topic}
            </li>
          );
        })}
      </ul>
    );
  };
  return (
    <div>
      <button
        onClick={() => {
          createRoom();
        }}
      >
        create Room
      </button>
      <h1>room list</h1>
      <List />
      <h1>message list</h1>
      {activeChannel && (
        <div>
          <div>
            <b>activeChannel:</b>
            <span style={{ color: "blue" }}>{activeChannel.topic}</span>
          </div>
          <div style={{ minHeight: 300, border: "1px solid #000" }}>
            {messageList.map((item) => {
              return <div key={item.id}>message: {item.content}</div>;
            })}
          </div>
          <div>
            <input
              value={text}
              type="text"
              onChange={(e) => {
                setText(e.target.value);
              }}
            />
            <button onClick={handleSendMessage}>send Message</button>
          </div>
        </div>
      )}
    </div>
  );
};

export default App;

```
