# RPC-node

The [yagna daemon](https://github.com/golemfactory/yagna) with private or public RPC calls makes it possible to enable all internet-operated devices (e.g. mobile phones, smart TV's, and more) to communicate with the Golem Network.

## Summary/abstract of the idea

To use Golem today, it's almost a requirement to download and operate your own daemon to run your own applications or requests. There have been some projects built to combat this, some major ones being GolemGrid & Slate. Neither of these projects however, try to do it whilst building up procedures and standards - which is a must for it to be easy to operate in a way that is not heavily controlled by a few people.

Turning yagna into something that can be opened up to internet solves this to an extent. As long as you know someone who operates a public daemon, or if you have someone who can lend you their private one, you will be able to use the Golem Network from more platforms and devices through, for example, WebJS.

This makes it super convenient to use Golem for web applications without having to put trust in anyone other than the node you're using for your one specific task.

> It's important to not see this as something new that has to be run, but rather an expansion of [yajsapi](https://github.com/golemfactory/yajsapi/) to work on WebJS remotely.

## A sample scenario/flow of the application

Perhaps you have a web application that is a bit too heavy for some devices to properly compute everything on their own. We can imagine that we are using something similar to the [Auto-Editor](https://auto-editor.online/) on Golem. Here, we're processing videos with the help of Golem, which can be very heavy on ones computer.

Right now, we're sending over our files to a server operated by a community member, which is then handled and sent over to the Golem network. Once this is finished, the result is sent back to the server, and you as an end-user can download the result.

But this does not favor decentralization very well. What happens if this community member stops maintaining their software and it suddenly stops working? Well that's easy, if it isn't open-source, the app is gone forever and nobody can use it. If it is open source, someone can self-host it, but that's tedious, annoying, and hard.

With Yagna RPC nodes, it would follow a different path. First, there can be WebJS libraries developed to work with the Yagna features in an easy fashion. Just like yapapi or yajsapi, we would have something (here running WebJS) to interact with the network in the browser.

After the backend workings are complete, it can be built into a web application and be hosted in different ways, either traditional or through "Web3 means." This includes something like Skynet, where applications can live on for longer than the developers wish to maintain them for.

Now, anyone just needs a link and they can access and use the web application. The code in this application does not have to rely on the user to run their own node, but can rather go through a list of known publicly hosted daemons and utilize those.

The user finds a node that works, and communicates with the Golem Network through that node, sending their video file over with all the required arguments. Just like a requestor script works locally, a requestor script can be within the browser as long as there is communication from the browser to a daemon.

Providers on the Golem Network receive the task as defined by the requestor script, and communicates with the RPC node, which in turn communicates with the requestor. 

The provider gets to know which scripts need to be run, which files to send back, and all data can be encrypted with X25519 to and from the provider for privacy against the RPC node.

Lastly, the RPC can forward everything, the payment can occur (manually/smart contracts/by trusting the RPC), and the user is finished. They have now interacted with the Golem Network from a frontend that is not reliant on a central authority, without the need of giving up their convenience or their privacy.
