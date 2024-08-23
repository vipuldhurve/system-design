# Content Delivery Network (CDN)
- A content delivery network (CDN) is a group of <b><i>geographically distributed servers that speed up the delivery of web content</i></b> by bringing it closer to where users are.
- CDNs rely on a process called <b><i>“caching”</i></b> that temporarily stores copies of files in data centers across the globe, allowing you to access internet content from a server near you
- Content delivered from a server closest to you reduces page load times and results in a faster, high-performance web experience.

You could think of a CDN like an ATM. If your money were only available from one bank in town, you’d have to make a time-consuming trip and stand in a long line every time you wanted to withdraw cash. However, with a cash machine on practically every corner, you have fast and easy access to your money any time you need it.
<div align="center">
  <img src="https://github.com/vipuldhurve/system-design/blob/main/assets/cdn.png" alt="Image" style="display:block; width:70%; height:auto; margin:auto;">
</div>

<br><br>
## How does a CDN work?
A content delivery network relies on three types of servers:
- <b><i>Origin servers:</i></b> Origin servers contain the original versions of content and they function as the source of truth. Whenever content needs to be updated, changes are made on the origin server. An origin server may be owned and managed by a content provider or it may be hosted on the infrastructure of a third-party cloud provider like Amazon’s AWS S3 or Google Cloud Storage.
- <b><i>Edge servers:</i></b> Edge servers are located in multiple geographical locations around the world, also called <b><i>“points of presence” (PoPs)</i></b>. The edge servers within these PoPs cache content that is copied from origin servers, and they are responsible for delivering that content to nearby users. When a user requests access to content on an origin server, they are redirected to a cached copy of the content on an edge server that’s geographically close to them. When cached content is out of date, the edge server requests updated content from the origin server. CDN edge servers are owned or managed by the CDN hosting provider.
- <b><i>DNS servers:</i></b> Domain Name System (DNS) servers keep track of and supply IP addresses for origin and edge servers. When a client sends a request to an origin server, DNS servers respond with the name of a paired edge server from which the content can be served faster.

#### To deliver the optimal viewing experience, CDNs perform two essential functions:
- <b><i>1. Reduce Latency:</i></b> Some content delivery networks alleviate latency by reducing the physical distance that the content needs to travel to reach you. Therefore, larger and more widely distributed CDNs are able to deliver website content more quickly and reliably by putting the content as close to the end user as possible.
- <b><i>2. Balance loads:</i></b> A CDN balances overall traffic to give everyone accessing internet content the best web experience possible. Think about it like routing traffic in the real world. There may be one route that’s usually the fastest from point A to point B if no other cars take it — but if it starts getting congested, it’s better for everyone if the traffic gets spread out over a few different routes. That may mean that you get sent on a roadway that’s a few minutes longer (or milliseconds, when scaled to internet speeds), but you don’t get stuck in the traffic jam that’s forming on the shortest route. Load balancing enables content providers to handle increases in demand and large traffic spikes while still providing high-quality user experiences and avoiding downtime.
<div align="center">
  <img src="https://github.com/vipuldhurve/system-design/blob/main/assets/how-cdn-work.png" alt="Image" style="display:block; width:85%; height:auto; margin:auto;">
</div>

<br><br>
## Types of CDNs
### 1. Push CDNs
Push CDNs receive new content whenever changes occur on your server. You are responsible for uploading content directly to the CDN and updating URLs to point to the CDN.

#### Push CDNs are ideal for:
- Sites with a small amount of traffic.
- Sites with content that doesn't change frequently.

#### Advantages of Push CDNs
- Content is uploaded only when it is new or changed, minimizing traffic
- Maximizes storage by keeping content until it expires or is updated.

### 2. Pull Cdns
Pull CDNs retrieve new content from your server when the first user requests it. The content remains on your server, and URLs are rewritten to point to the CDN. The initial request may be slower until the content is cached on the CDN.

#### Pull CDNs are suitable for:
- Sites with heavy traffic.
- Scenarios where only recently-requested content remains cached on the CDN.

#### Advantages of Pull CDNs
- Minimizes storage space on the CDN.
- Distributes traffic evenly across servers.

<br><br>
## Benefits of a CDN
### 1. Boost performance
- Performance is the difference between a click giving you immediate access to new content and a click followed by a seven-second wait while a page loads or a video buffers. That wait time is called **“buffering”** and is symbolized by a familiar swirling circle icon on the screen. 
- To ensure high performance and minimize buffering, CDNs deliver content that’s been pre-saved on nearby servers on the CDN’s network rather than sending requests to origin servers which may be halfway around the world.
<div align="center">
  <img src="https://github.com/vipuldhurve/system-design/blob/main/assets/cdn-traffic-load.jpg" alt="Image" style="display:block; width:80%; height:auto; margin:auto;">
</div>

### 2. Offload traffic
- With the explosive growth of online streaming and other rich media services, and higher user expectations about web performance across multiple device types, many of today’s network service providers are finding their content distribution networks to be highly stressed.
- By responding to a request for web content with a cached version from servers closer to the end user, a CDN can offload traffic from content servers and improve the web experience

### 3. Ensure availability
- Availability means that content remains accessible to end users even during periods of excessive user traffic when many people are accessing content at the same time or if there are server outages in some parts of the internet. 
- Advanced CDNs, with their highly distributed architecture and massive platform of servers, can absorb 100+ Tbps of traffic and make it possible for content providers to stay available to even larger user bases.

### 4. Enhance Security
- CDNs can also improve website security with increased protection against malicious actors and threats like distributed denial-of-service (DDoS) attacks.
- Today’s most advanced content delivery networks provide unique cloud-based security solutions and DDoS protection.

### 5. Improve customer experiences
- Content, application, and website owners — including ecommerce sites, media properties, and cloud computing companies — use CDNs to improve customer experiences, lower abandonment rates, increase ad impressions, improve conversion rates, and strengthen customer loyalty.

### 6. Reduce bandwidth costs
- By delivering content from servers closer to users, CDNs reduce bandwidth consumption and the associated costs.

### 7. Gather intelligence
- As carriers of nearly half of the world’s internet traffic, CDN providers generate vast amounts of data about end-user connectivity, device types, and browsing experiences across the globe.
- This data can provide CDN customers with critical, actionable intelligence and insights into their user base.
- Intelligence from CDNs also enables services such as real user monitoring, media analytics that measure end-user engagement with web content, and cloud security intelligence to keep track of online threats.

<br><br>
#### #Resource: [www.akamai.com/glossary/what-is-a-cdn](https://www.akamai.com/glossary/what-is-a-cdn)