# Using your domain to access service

You can easilly set up routing rules — which domains and their locations should point to which service inside your [project]().

![Domain add](/domain-add.png "Domain add")

All you need to do it point your domains A and / or AAAA records to one of the [unique IP addresses]() assigned to your [project]().

**[Let's encrypt](https://letsencrypt.org/) SSL Certificate can be automatically installed for you.**

![Domains](/domains.png "Domains")

**Domain access can be enabled only on services running on HTTP ports**. Internally Zerops points all traffic to a L7 HTTP Balancer that takes care of SSL certificate installation and load balancing traffic to selected container running your app.

![L7 balancer](/l7balancer.png "L7 balancer")
