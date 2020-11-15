### Reliable, Scalable, and Maintainable Applications


#### Reliability
The system should continue to work correctly (performing the correct function at the desired level of performance) even in the face of adversity (hardware or soft‐ ware faults, and even human error).
#### Scalability
As the system grows (in data volume, traffic volume, or complexity), there should be reasonable ways of dealing with that growth.
#### Maintainability
Over time, many different people will work on the system (engineering and oper‐ ations, both maintaining current behavior and adapting the system to new use cases), and they should all be able to work on it productively. 

#### Reliability (“continuing to work correctly, even when things go wrong.”)
    - The application performs the function that the user expected.
    - It can tolerate the user making mistakes or using the software in unexpected ways.
    - Its performance is good enough for the required use case, under the expected load and data volume.
    - The system prevents any unauthorized access and abuse.
  
The things that can go wrong are called faults, and systems that anticipate faults and can cope with them are called *fault-tolerant* or *resilient*.
   
   Error can come from hardware errors, software errors, human errors
   
   
#### Scalability （the term we use to describe a system’s ability to cope with increased load.）
How to describe the performance of a system?


  - *throughput*—the number of records we can process per second, or the total time it takes to run a job on a dataset of a certain size。
  - *response time*—the time between a client sending a request and receiving a response.
      - The response time is what the client sees: besides the actual time to process the request (the service time), it includes network delays and queueing delays. 
      - Latency is the duration that a request is waiting to be handled—during which it is latent, awaiting service
      
  - High percentiles of response times, also known as *tail latencies*, are important because they directly affect users’ experience of the service. 
      
