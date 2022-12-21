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

```tsx
const connect = async () => {
  const res = await ethereum.request({
    method: 'wallet_enable',
    params: [
      {
        wallet_snap: {
          'npm:web3-mq-snaps': {
            version: '1.0.6',
          },
        },
      },
    ],
  });
  console.log(res, 'connectSnaps');
};

<button onClick={connect}>connectSnaps</button>;
```

After the installation is complete you need a method to execute all the instructions uniformly, let's declare such a utility function

```ts
async function send(method, params) {
  try {
    const res = await window.ethereum.request({
      method: 'wallet_invokeSnap',
      params: [
        'npm:web3-mq-snaps',
        {
          method, 
          params,
        },
      ],
    });
    console.log(res, 'response');
    return res;
  } catch (err) {
    alert(`Problem happened: ${err.message}` || err);
  }
}
```

## Initialize Client and Connect to Web3MQ Network

In order to connect to the Web3MQ Network, both users and developers authenticate through wallet signatures, we demonstrate below with an Ethereum wallet via Metamask, but Web3MQ is built to be compatible with wallets across different chains.

#### API endpoints

During this initial testing phase, we've hosted complete networks of Web3MQ nodes in different regions around the globe. Connect to these endpoints below, to access the Web3MQ Testnet.

- https://testnet-us-west-1-1.web3mq.com
- https://testnet-us-west-1-2.web3mq.com
- https://testnet-ap-jp-1.web3mq.com
- https://testnet-ap-jp-2.web3mq.com
- https://testnet-ap-singapore-1.web3mq.com
- https://testnet-ap-singapore-2.web3mq.com

### Sign with wallet to register user and obtain message encryption keys

For any first-time user of Web3MQ's network, you'll need to register on Web3MQ's network. mq-web3 snap takes care of the key generation process and subsequent wallet signing process. execution register command to get keys

## Create room

After initializing the client and registering your user, the next step is to connect to a room

#### Code

```ts
const createRoom = async function () {
  await send('creatRoom');
};
```

```tsx
<button onClick={createRoom}>createRoom</button>
```

## Get Channel List

#### Code

```ts
const GetChannelList = async function () {
  const channelListArr = await send('queryChannelList', {
    options: { page: 1, size: 100 },
  });

  console.log(channelListArr);
};
```

```tsx
<button onClick={GetChannelList}>GetChannelList</button>
```

## Send Message

#### Code

```ts
const sendMessage = async function () {
 await send('sendMessage', { msg: 'testMsg', topic: 'Select the topic you got from GetChannelList API' })
};
```

```tsx
<button onClick={sendMessage}>sendMessage</button>
```

## Get Message List

#### Code

```ts
const getMessageList = async function () {
 await send('getMessageList', { options: { page: 1, size: 100 }, topic: 'Select the topic you got from GetChannelList API' })
};
```

```tsx
<button onClick={getMessageList}>getMessageList</button>
```

## Full Example

- https://github.com/Generative-Labs/Web3-MQ-snap-demo
