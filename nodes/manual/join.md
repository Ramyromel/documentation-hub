---
title: Join Network
hide_table_of_contents: false
---

<head>
  <title>Join the HyperGraph or State Channel</title>
  <meta
    name="description"
    content="This document will help to join an existing HyperGraph Network or State Channel."
  />
</head>

We are now ready to join our State Channel and/or HyperGraph network.

:::info NOTE
Our example will join the **testnet 2.0** network.
:::

## Instructions

### Log into your node

From your **local system**, log into your **cloud instance's** terminal as **nodeadmin** using your Apple terminal, Window's PuTTY, or your terminal application of choice.

:::note
You can remind yourself how to access your VPS here for [Macintosh](/nodes/resources/accessMac) or [Windows](/nodes/resources/accessWin).
:::

### Update your node
Bring our Node up to date

```
sudo apt -y update && sudo apt -y upgrade
```

You will be prompted for your **`nodeadmin`** password.

:::warning
Your screen will not react and your password will not show as you type.  
**Reminder**: `[...]` in the output command examples means that there is a bunch of output that has been redacted to eliminate confusion. 
:::

```
[sudo] password for nodeadmin:
[...]
```

### Start the Join Process

:::note CURRENTLY
We have created our **VPS**, installed out **dependencies**, installed out **Tessellation binaries**, setup our **user**, created our **p12** file, and our **service** file.
:::

We will send a request to the Source node to join the cluster, through a **`curl POST`** request.

:::tip IMPORTANT
The current example will help us to join the **Constellation Network Tessellation TestNet** if you are going to join a different State Channel or HyperGraph network, you will need to obtain the proper **node id** and **ip** address, and substitute them into the commands below
:::

### Supply your P12 passphrase

:::danger IMPORTANT
We do **not** want to have our p12 **passphrase** added to a static plain text file.  Our **p12** file is our private key file that stores valuable information.  If the passphrase is exposed, you can have access to the MainNet, State Channel, TestNet, etc. compromised, including access to wallets.  This is a **bad** idea.
:::

:::note NOTE
In previous steps, we started our **node service**.  This service will **not** properly start without your **p12 passphrase**.  The command below may **not** be necessary; however, we will enter it again to be thorough 
:::

Instead, we will create a *temporary* environment variable prior to joining the network.  The export we do below will only survive the current working session, and it will be lost after we log out.  

```
export CL_PASSWORD="place_your_passphrase_here"
```

### Join the network

Next our command to **join** is:
```
curl -X POST http://127.0.0.1:9002/cluster/join -H 'Content-type: application/json' -d '{ "id": "e2f4496e5872682d7a55aa06e507a58e96b5d48a5286bfdff7ed780fa464d9e789b2760ecd840f4cb3ee6e1c1d81b2ee844c88dbebf149b1084b7313eb680714", "ip": "13.52.246.74", "p2pPort": 9001 }'
```

We should now be joined to the network. Lets check to see if we are connected by Checking our Peers.

Open your browser on your local computer.

```
http://<ip_address_of_your_instance>:9000/cluster/info
```
:::info MAKE SURE
We need to replace `<ip_address_of_your_instance>` with your Node's ip address.
:::

```
http://111.111.111.111:9000/cluster/info
```

You will see a list of lines with **`id`**, **`ip`**, **`publicPort`**, **`p2pPort`**, **`session`**, **`state`**.

The content is not important, what is important is that you see at least 1 entry display in your local browser window.

:::note
The **`"id"`** fields in this example are randomly made up improperly formatted id fields used for the purpose of `example` only.
:::

```
[{"id":"11111111111111111aaaaaaaaaaaaaaaaaaaaaaabbbbbbbbbbbbbbbbbccccccccccccccddddddddddddddddddddeeeeeeeeeeeeeeeeeeeeeeeeffffffffffffff","ip":"111.111.111.111","publicPort":9000,"p2pPort":9001,"session":"aaaaaaaa-bbbb-cccc-dddd-ffffffffffff","state":"Ready"},
{"id":"ggggggggggggggggggggghhhhhhhhhhhhhhhhhhhhhiiiiiiiiiiiiiiiiiiiiiijjjjjjjjjjjjjjjjjjjjjkkkkkkkkkkkkkkkkkkklllllllllllllllllmmmmmmmmm","ip":"122.222.222.222","publicPort":9000,"p2pPort":9001,"session":"gggggggg-hhhh-iiii-jjjj-kkkkkkkkkkkk","state":"Ready"},
{"id":"nnnnnnnnnnnnoooooooooooooooopppppppppppppppppppqqqqqqqqqqqqqqqqqqqqrrrrrrrrrrrrrrrrrrrrrrrrrrrrrrrsssssssssssssssstttttttttttttttt","ip":"133.333.333.333","publicPort":9000,"p2pPort":9001,"session":"llllllll-mmmm-nnnn-oooo-pppppppppppp","state":"Ready"}
[...]
```

### Optional: review your logs

```
cat /var/tessellation/logs/app.log
```

You should see somewhere towards the end of the log file a line which contains:
```
Received state=Ready{ }
```

:::success MULTIPLE STATE CHANNELS
In the event that you would like to participate in **multiple** state channels, you can issue the join separately for each state channel process you have running on the Node.
:::