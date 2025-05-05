# System Design

[docs](https://github.com/donnemartin/system-design-primer?tab=readme-ov-file#system-design-topics-start-here)

[docs](https://www.youtube.com/watch?v=-W9F__D3oY4)

## Vertical Scaling

This is basically throwing more resources/money to the bottleneck. e.g. Buying a machine with more CPU or GPU.

## Horizontal Scaling

Instead of getting one really good machine, you can use multiple cheaper older servers. The issue now is that an inbound request will now need to be distributed across to a server.

### Load Balancer

The load balancer is a 'man in the middle' that balances loads. Instead of in DNS returning the IP address of the `public IP address` of the server, the rest of the world only sees the IP address of the load balancer, and the load balancer figures out what server to send the request to. The servers would then have `private IP addresses`.

![load balancer](./load%20balancer.png)

The load balancer figures out who to send to based on `load` or busyness uses TCP/IP to send the request to the server. It can figure out the server based on load like CPU or RAM usage by talking to the servers. Otherwise, your servers can do specific tasks, i.e. image processing and one for file transfers. Or it can be a fancy DNS server which returns the IP address of server 1 for the first request, then IP address of server 2 for the second request. This is called `Round Robin`.

#### Round Robin

![round robin](./Round%20robin.png)

A very popular DNS server for this is `BIND`.

<!-- 24mins -->

## Clones

[docs](https://web.archive.org/web/20220530193911/https://www.lecloud.net/post/7295452622/scalability-for-dummies-part-1-clones)
