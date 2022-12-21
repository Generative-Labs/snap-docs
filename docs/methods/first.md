---
sidebar_position: 1
---


# Methods
### creatRoom()
```ts
const createRoomsBySnaps = async (roomName: string = "") => {
  return await ethereum.request({
    method: "wallet_invokeSnap",
    params: [
      newSnapId,
      {
        method: "creatRoom",
        payload: { group_name: roomName },
      },
    ],
  });
};
```
### queryChannelList()
```ts
const getChannelListBySnaps = async () => {
  //@ts-ignore
  return await ethereum.request({
    method: "wallet_invokeSnap",
    params: [
      newSnapId,
      {
        method: "queryChannelList",
        payload: { options: { page: 1, size: 100 } },
      },
    ],
  });
};
```
### getTargetUserId()
```ts
const getUserIdByAddress = async (address: string) => {
    //@ts-ignore
    return await ethereum.request({
        method: "wallet_invokeSnap",
        params: [
            newSnapId,
            {
                method: "getTargetUserId",
                payload: address,
            },
        ],
    });
}
```
### sendMessage()
```ts
const sendMessageBySnaps = async (msg: string, topic: string) => {
  //@ts-ignore
  return await ethereum.request({
    method: "wallet_invokeSnap",
    params: [
      newSnapId,
      {
        method: "sendMessage",
        payload: { msg, topic },
      },
    ],
  });
};

```
### getMessageList()
```ts
const getMessagesBySnaps = async (topic: string) => {
    //@ts-ignore
    return await ethereum.request({
        method: "wallet_invokeSnap",
        params: [
            newSnapId,
            {
                method: "getMessageList",
                payload: {
                    options: { page: 1, size: 100 },
                    topic,
                },
            },
        ],
    });
}

```
### sendNotifyMessage()
```ts

export const sendNotifyMessage = async (message: string) => {
    //@ts-ignore
    return await ethereum.request({
        method: "wallet_invokeSnap",
        params: [
            newSnapId,
            {
                method: "sendNotifyMessage",
                payload: { message },
            },
        ],
    });
};
```

# Commands