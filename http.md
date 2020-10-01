### Cookies
HTTP is stateless, but cookies are introduced to have a form of state persistence. The server sends the data to the user's browser, which stores it locally until the server (or another) requests for it later.

### Performance of HTTP
Measured by Page Load Time (PLT), from when the user clicks until the page appears on screen.

It is dependent on page content, the protocols involved as well as network bandwidth.

**Goals:**
- Fast downloads
- High availability
- Cost affective infrastructure
- Avoid overload

We can achieve points 1, 2, 4 through caching. However, in order to lower costs for infrastructure, we can exploit economies of scale in the form of web hosting, CDNs and data centers.

### Caching
**Proxies**
A server which can service requests without the user having to communicate with the _origin_ server. To do this, the server has to act as a web server to the user and also a client to the origin server.