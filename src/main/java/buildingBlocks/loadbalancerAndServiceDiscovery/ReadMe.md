# Load Balancer
A load balancer is a device or an application that distributes the network traffic across multiple servers or instances to ensure no single server becomes overwhelmed.This helps in improving the performance, responsiveness, availability, reliability of application.
<div align="center">
  <img src="https://github.com/vipuldhurve/system-design/blob/main/assets/load-balancer.png" alt="Image" style="display:block; width:90%; height:auto; margin:auto;">
</div> 

### Key Functions:
- <b>Traffic Distribution:</b> Distributes incoming requests to multiple backend servers based on various algorithms like round-robin, least connections, IP hash, etc.
- <b>Health Checks:</b> Regularly checks the health of backend servers and ensures traffic is only directed to healthy instances.
- <b>Sticky Sessions:</b> Ensures requests from the same client are directed to the same server for session consistency. 
- <b>SSL Termination:</b> Offloads the SSL/TLS encryption and decryption from backend servers, improving their performance.

### Types:
- <b>Hardware Load Balancers:</b> Physical devices designed for high performance and reliability.
- <b>Software Load Balancers:</b> Applications that run on general-purpose hardware, offering more flexibility and easier integration with cloud environments.

### Traffic Routing
Load balancers can route traffic based on various metrics, including:
- Random
- Least connections/loaded
- Session/cookies
- Round-robin or weighted round-robin
- IP Hash(Layer 4 or Layer 7)

#### Examples
- NGINX
- HAProxy
- AWS Elastic Load Balancer (ELB)
- Google Cloud Load Balancing

<br><br>
# Service Descovery
Service discovery is a mechanism that enables services to find and communicate with each other within a distributed system. It automates the process of detecting and tracking service instances, making it easier to manage the dynamic and scalable environments.

### Key Functions:
- <b>Service Registration:</b> Services register themselves with a service registry when they start up and deregister when they shut down.
- <b>Service Lookup:</b> Clients query the service registry to find the addresses of available service instances.
- <b>Health Monitoring:</b> Regularly checks the health of the registered services and updates their status in the service registry.

### Types:
- <b>Client-Side Discovery:</b> The client requires the service registry to find the addresses of available service instances.
<div align="center">
  <img src="https://github.com/vipuldhurve/system-design/blob/main/assets/client-side-service-registry.png" alt="Image" style="display:block; width:50%; height:auto; margin:auto;">
</div> 

- <b>Service-Side Discovery:</b> A load balancer queries the service registry and directs the client requests to the appropriate service instances.
<div align="center">
  <img src="https://github.com/vipuldhurve/system-design/blob/main/assets/server-side-service-registry.png" alt="Image" style="display:block; width:60%; height:auto; margin:auto;">
</div> 

#### Examples
- Consul
- Eureka (Netflix)
- Zookeeper
- Etcd
<br><br>
## Interaction between Load Balancer and Service Registry
In a microservices architecture, load balancers and service discovery mechanisms often work together. Here’s how they typically interact:
- <b>Service Registration:</b> When a service instance starts, it registers its address (e.g., IP and port) with the service registry.
- <b>Service Health Check:</b> The service registry performs health checks on registered instances and updates their status
- <b>Client Requests:</b> When a client needs to communicate with a service, it either queries the service registry directly (client-side discovery) or sends a request to the load balancer.
- <b>Load Balancer Request Routing</b> The load balancer queries the service registry to get the addresses of healthy service instances and routes the client’s request accordingly.
<div align="center">
  <img src="https://github.com/vipuldhurve/system-design/blob/main/assets/load-balancer-and-service-registry.png" alt="Image" style="display:block; width:80%; height:auto; margin:auto;">
</div> 

The load balancer interacts with the service discovery tool to get the list of available service instances and their health status, ensuring that traffic is routed only to healthy instances. This combination ensures efficient distribution of traffic, high availability, reliability, and seamless scaling of services.
