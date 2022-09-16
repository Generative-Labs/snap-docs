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
            version: '1.0.2',
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
async function send(method, payload) {
  try {
    const res = await window.ethereum.request({
      method: 'wallet_invokeSnap',
      params: [
        'npm:web3-mq-snaps',
        {
          method,
          payload,
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

### Connect to Web3MQ Network

:::note

While we are committed to building an open and collectively owned public good, our early stage testnet requires an API key in order to connect. This is to control capacity to make sure that each early partner and developer is able to build a great experience on top of Web3MQ. [Apply here](https://web3mq.com/apply).

:::
We use the unified command execution method send we declared above to connect to our Web3MQ network.Simply execution init command without connectUrl or an empty string returns a url of the best node determined for you, and this url can be stored locally.

```tsx
const initWeb3MQ = async function () {
  // You can save the bestEndpointUrl locally to skip endpoint search next time, which will save time
  const bestEndpointUrl = await send('init', {
    connectUrl: '',
    app_key: 'app_key', // temporary authorization key obtained by applying, will be removed in future testnets and mainnet
  });
  console.log('bestEndpointUrl', bestEndpointUrl);
};
```

execution init command with a specific connectUrl forces the client to connect to that specific node. When bestEndpointUrl is stored, it might be time-saving to connect directly instead of running the search again.

```ts
const initWeb3MQ = async function () {
  const bestEndpointUrl = await send('init', {
    connectUrl: bestEndpointUrl, // takes in a valid endpoint url as input, when this paramter is given, client will always connect to that specific node.
    app_key: 'app_key', // Appkey applied from our team
  });
};
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

For any first-time user of Web3MQ's network, you'll need to register on Web3MQ's network. mq-web3 snap takes care of the key generation process and subsequent wallet signing process. execution register command to get keys

#### Code

:::note

You must ensure that execution init command and make sure it is complete before running this
:::

```ts
const register = async function () {
  // You must ensure that the initWeb3MQ initialization is complete before running this
  const keys = await send('register', {
    signContentURI: 'https://www.web3mq.com', // your signContent URI
    EthAddress: 'your eth address', // *Not required*  your eth address, if not use your MetaMask eth address
  });
  const { PrivateKey, PublicKey } = keys;

  // Keep your private key in a safe place, this is for demo purposes only
  localStorage.setItem('PRIVATE_KEY', PrivateKey);
  localStorage.setItem('PUBLICKEY', PublicKey);
};
```

### Get Client Instance

#### Code

```ts
const keys = { PrivateKey, PublicKey };

const getClientInstance = async function () {
  await send('getInstance', keys);
};
```

```tsx
<button onClick={getClientInstance}>get Client Instance</button>
```

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
 await send('sendMessage', { msg: 'testMsg', topic: 'Select the topic you got from GetChannelList API' }),
};
```

```tsx
<button onClick={sendMessage}>sendMessage</button>
```

## Get Message List

#### Code

```ts
const getMessageList = async function () {
 await send('getMessageList', { options: { page: 1, size: 100 }, topic: 'Select the topic you got from GetChannelList API' }),
};
```

```tsx
<button onClick={getMessageList}>getMessageList</button>
```

## Full Example

- https://github.com/Generative-Labs/web3-mq-snaps-demo
