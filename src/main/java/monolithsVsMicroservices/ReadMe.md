## Monolithic Architecture
- Monolithic architecture is a application development technique where everything is constructed in a single unit or block called monolith.
- All components are tightly coupled into a single cohesive unit.
- A monolithic application will use one codebase and one runtime environment to create an application.
- This means that multiple services like database, api's etc will function as one large application organized. However, all the components as well as any associated components needs to be present for the software to successfully run.

### Advantages of Monolithic Architecture
- <b><i>Simple development:</i></b> A small team can rapidly pull together and build an executable app using a monolithic system. This makes monolithic architecture ideal for startups without big software development budgets.
- <b><i>Easy deployment:</i></b> Monolithic architecture is not as complex as microservices. It has fewer moving parts, so there are fewer components to manage and fix.
- <b><i>Uncomplicated testing and debugging:</i></b> Because the application is fitted as a single unit and works together as a whole, you can perform monolithic architecture testing and debugging quickly and easily from a central logging system.

### Disadvantages of Monolithic Architecture
- <b><i>Less scalability:</i></b> Because monolithic architecture software is tightly coupled, it can be hard to scale. If you want to add new features or your codebase grows, you will need to scale up or down the entire architecture.
- <b><i>Inability to adapt to new technologies:</i></b> As mentioned before, monolith applications are tightly coupled. This also means it’s hard to bring in new technologies or web services without dismantling the entire app.
- <b><i>High dependence between functionalities:</i></b> if one feature or functionality down, the others will go down with it. 

## Microservices Architecture
- Microservices approach creates a large application out of multiple, modular services (microservice or a service) that communicate with each other via API's.
- Each service work on a specific business goal.
- They are loosely coupled, independently deployable and use independent database and codebase.
- An individual team is typically responsible for each service used in a microservice architecture that follows developing, testing, deploying and scaling individually.

### Advantages of Microservices Architecture
- <b><i>Independent services:</i></b> In microservices architecture, each service is developed independently of the others, with the business logic spread over various platforms. This means workflows for one service don’t affect the development of others. This enables quick and efficient independent development
- <b><i>Enables agile development:</i></b> As each service is developed independently, you can choose the tech stack and programming language that best suits each function. This kind of agility allows applications to be developed more quickly and efficiently.
- <b><i>Scalable:</i></b> Because of each function’s loose coupling, it’s easy to optimize, test, debug, and fix functions independently of one another. Rather than putting the whole app into downtime and scaling each service, you can instead tweak and upgrade independent services as and when needed.
- <b><i>Reliable:</i></b> This loose coupling makes microservice architecture services more reliable. One crashed service doesn’t bring down the entire app. If something goes wrong with an API gateway, the other services can still operate independently.

### Disadvantages of Microservices Architecture
- <b><i>Complex & Complicated :</i></b> Deploying a microservice architecture isn’t always easy, and it gets harder the more complex your app is. Every service in a microservice application has to be tested individually. Once all the individual service tests have been completed, you need to test how they work as a whole.
- <b><i>High Infra Cost:</i></b> Each service has his own database and resources which makes it costly.
- <b><i>Management overhead:</i></b> Need proper management between teams and services.

<div align="center">
  <img src="https://github.com/vipuldhurve/system-design/blob/main/assets/monolith%20vs%20microservices.png" alt="Image" style="display:block; margin:auto;">
  <p style="position:absolute; bottom:0; text-align:center; width:100%;">Monolith v/s Microservices</p>
</div>


## FAQs 

#### Q. How microservices communicate with each other?
- <b>Synchronous - HTTP/ HTTPS:</b> Microservices often communicate over the HTTP or HTTPS protocols using RESTful APIs. Each microservice exposes endpoints that other services can call to request or manipulate data. This approach is straightforward and widely used, making it suitable for many scenarios.
- <b>Asynchronous - Messaging Queues:</b> Microservices can communicate asynchronously through messaging queues like RabbitMQ, Apache Kafka, or Amazon SQS. With messaging queues, services can publish messages to a queue, and other services can consume these messages.
- <b>RPC (Remote Procedure Calls):</b> Microservices can use RPC mechanisms such as gRPC or Thrift to communicate with each other. RPC allows services to call methods or procedures on remote services as if they were local, abstracting away the network communication complexity. gRPC, for example, offers high-performance communication using protocol buffers and HTTP/2.
- <b>Event Streaming::</b> Some microservices architectures rely on event streaming platforms like Apache Kafka or Amazon Kinesis. In this approach, services produce and consume events, enabling real-time communication and data processing. Event streaming is particularly useful for scenarios where events need to be processed asynchronously and distributed across multiple services.
- <b>Service Mesh:</b> Service meshes provide features like service discovery, load balancing, encryption, and observability, enhancing the reliability and security of microservices communication.
<br><br>

#### Q. Is it possible to use a hybrid of monolithic and microservices?
Ans. The short answer is yes, a hybrid approach, which combines the benefits of both monolithic and microservices architectures, is possible.

In a hybrid architecture, certain parts of the application are designed and implemented as microservices, but the core functionality remains within a monolithic structure.
<br><br>

#### Q. Are communications between microservices secure?
Ans. The security of communications between microservices depends on the implementation and configuration of the communication mechanisms. While the communications between microservices are not inherently secure, measures can be implemented to protect data and guarantee security.

Some key considerations for ensuring secure communications between microservices include authentication and authorization, encryption, secure communication protocols, network segmentation, firewalls, and continual monitoring and auditing.


