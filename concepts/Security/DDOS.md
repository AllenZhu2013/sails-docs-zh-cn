# DDOS
拒绝服务攻击([denial of service attacks](https://www.owasp.org/index.php/Application_Denial_of_Service))的保护是一个[复杂棘手的问题](http://en.wikipedia.org/wiki/Denial-of-service_attack#Handling)，牵扯到了网络协议栈自上而下的多层保护。这种类型的攻击在最近这几年[臭名远扬](http://www.darkreading.com/vulnerabilities-and-threats/10-strategies-to-fight-anonymous-ddos-attacks/d/d-id/1102699)因为广泛的媒体报道。

<<<<<<< HEAD
在API层，在保护的措施上没有多少可坐的。然而，Sails提供一些设置可以减轻DDOS攻击：
+ 在Sails中的session可以[配置成](http://sailsjs.org/documentation/reference/sails.config/sails.config.session.html)使用一个独立的session仓库(比如[Redis](http://redis.io/))，允许你的应用运行在不依赖任何一个API服务器的内存状态中。这意味着你的Sails app的多份拷贝可能部署成很多台需要处理流量的服务器。(这个通过使用[负载均衡器](http://en.wikipedia.org/wiki/Load_balancing_computing)来实现，可以直接将每次请求转给资源空闲的服务器并处理请求，消除任何一个app 服务器的单店故障的风险。
+ Socket.io连接可以[配置成](http://sailsjs.org/documentation/reference/sails.config/sails.config.sockets.html)使用一个独立的[socket store](https://github.com/balderdashy/sails-docs/blob/master/PAGE_NEEDED.md)(比如Redis)，用来管理发布/订阅状态和消息队列。这个可以消除在负载均衡器上严格sessions的需求，防止攻击者zhidao指导他们一遍又一遍地攻击相同服务器。
=======
The prevention of [denial of service attacks](https://www.owasp.org/index.php/Application_Denial_of_Service) is a [complex problem](http://en.wikipedia.org/wiki/Denial-of-service_attack#Handling) which involves multiple layers of protection, up and down the networking stack.
This type of attack has achieved [notoriety](http://www.darkreading.com/vulnerabilities-and-threats/10-strategies-to-fight-anonymous-ddos-attacks/d/d-id/1102699) in recent years due to widespread media coverage of groups like Anonymous.

At the API layer, there isn't much that can be done in the way of prevention.  However, Sails offers a few settings to mitigate certain types of DDOS attacks:

+ The session in Sails can be [configured](http://sailsjs.com/documentation/reference/sails.config/sails.config.session.html) to use a separate session store (e.g. [Redis](http://redis.io/)), allowing your application to run without relying on the memory state of any one API server.  This means that multiple copies of your Sails app may be deployed to as many servers as is necessary to handle traffic.  This is achieved by using a [load balancer](http://en.wikipedia.org/wiki/Load_balancing_(computing)), which directs each incoming request to a free server with the resources to handle it, eliminating any one app server as a single point of failure.
+ Socket.io connections may be [configured](http://sailsjs.com/docs/reference/configuration/sails-config-sockets) to use a separate [socket store](sailsjs.com/docs/concepts/deployment/scaling) (e.g. Redis) for managing pub/sub state and message queueing. This eliminates the need for sticky sessions at the load balancer, preventing would-be attackers from directing their attacks against the same server again and again.

> Note that, if you have the long-polling transport enabled in [sails.config.sockets](http://sailsjs.com/docs/reference/configuration/sails-config-sockets), you'll still want to make sure TCP sticky sessions are enabled at your load balancer.  For more on that, check out this writeup about [sockets on Deis and Kubernetes](https://deis.com/blog/2016/socket.io-applications-kubernetes/).

>>>>>>> upstream/master

### Additional Resources
+ [Backpressure and Unbounded Concurrency in Node.js](http://engineering.voxer.com/2013/09/16/backpressure-in-nodejs/) ([Voxer](http://voxer.com/))
+ [Building a Node.js Server That Won't Melt](https://hacks.mozilla.org/2013/01/building-a-node-js-server-that-wont-melt-a-node-js-holiday-season-part-5/) ([Mozilla](https://hacks.mozilla.org/))
+ [Security in Node.js](https://www.harrytorry.co.uk/node-js/security-flaws-in-node-js/) - see the "Denial of Service" section ([Harry Torry](https://www.harrytorry.co.uk))
+ [Slowloris DDoSAttacks](http://www.ddosattacks.biz/attacks/slowloris-ddos-attack-aka-slow-and-low/)



<docmeta name="displayName" value="DDOS">
