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

#### DNS Round Robin

![round robin](./Round%20robin.png)

A very popular DNS server for this is `BIND`. This is a problem when one server might have more heavy weight users, and so will have even more users even when it is at capacity.

There is another issue where the IP address of the DNS response is cached on your computer and browser, so it may hit the same server.

### Shared Session State

If you are have session and your state is stored on a server, then your stored session details will not be returned from a different server if load balancing.

This can be solved by moving the session state to a particular server or to the load balancer. However, it removes your redundancy if your server breaks down. Redundancy can be added back in if using `shared session storage` such as `RAID`.

This can be solved through saving the id of the server on a `cookie`. This can be a UUID instead of a private IP address.

#### Shared Session Storage

Using storage to solve shared sessions.

### Redundant Array of Independent Disks (RAID)

This is a computer that has multiple hard drives and then when the OS writes data, for RAID0 it `stripes` which writes a bit in the first hard drive and then a bit to the second hard drive.

For RAID1, it'll be written to both drives, where one might fail so the other patches it up.

For RAID10, it is uses 4 drives and mixes `stripes` and duplication.

This can still fail if the power goes out. So you can still use multiple servers and just sync the data together using `replication`.

### Inmemory Caching

It is very efficient to just serve a HTML files from a server. This can be cached easily via scripting languages, e.g. the compiled code (.pyc) in Python. You can also cache SQL queries. You can also cache using `memcache` which caches in RAM instead of the disk. If the cache becomes too big for your RAM or disk space, you'll implement garbage collection based on expiry.

### Database Replication

This is replication of databases.

#### Active-Passive

This is when one database is replicated to all of its 'slaves'. It solves redundancy issues if the Active database dies then the one of the other databases can be promoted to take its place. It can also be used to balance read requests for them where the passives are used for reads and the writes go to the active.

![Active Passive Replication](./Database%20Replication%20-%20Master%20Slave.png)

A combined diagram of the load balancer with database replication becomes like this:

![Load Balancer with db replication](./Load%20balancer%20and%20db%20replication.png)

The issues with this is the Load balancers are still single points of failure. Typically though, they come in pairs and may be Active-Passive or Active-Active and communicate their liveness with a `heartbeat` (a packet that is routinely sent to the other server). This is called `High Availability`.

#### Active-Active

This is more expensive but it fixes an issue where you have multiple active servers. Writing to Active 1 will get propagated to Active 2 and vice versa. This ensures that when one active server fails, then writes will still write.

### Partitioning

This is when you partition one type of data and store them in different load-balanced databases e.g. in old facebook this would be mit.facebook.com vs harvard.facebook.com. However, it would be very difficult if you try to do something that would cross the partitions.

![Partitions](./Partitions.png)

However, they instead did this by partitioning by names A-M and N-Z.

## Availability Zones

If your entire network is in a building, and the building loses power, then it can be mitigated by copying the data centers elsewhere.

Load balancing is now done on the DNS level.

## Security

It is good to only allow tcp port 80 (HTTP) and 443 (HTTPS) as well as a SSL based VPN or port 22 (SSH) for SSSHing into your data center. Once you are inside the data center, you can then keep communications open to HTTP to avoid complexity. From the load balancer for the databases, the communications would be tcp 3306 (MySql).

## Clones

[docs](https://web.archive.org/web/20220530193911/https://www.lecloud.net/post/7295452622/scalability-for-dummies-part-1-clones)
