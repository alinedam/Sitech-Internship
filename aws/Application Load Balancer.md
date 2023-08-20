# High availability and scalability

- scalability :means that an application /system can handler greater loads by adapting
- there are to kinds of scalability :
    - vertical
    - horizontal
- scalability linked but different to High Availability

## vertical scalability

- increasing the size of an instance
- for example our application runs on a t2.micro
- scaling that application vertically  running it on t2.large
- vertical scalability is very common for non distributed systems, such as databases
- RDs, Elastic Cache are services that can scale vertically

## Horizontal scalability

- horizontal scalability means increasing the number of instances / systems for your application
- horizontal scaling implies distributed systems
- this is very common for web applications/ modren application

# High availability

- high availability usually goes hand in hand with horizontal scaling
- high availability means running your application / system in at least 2 data centers
- the goal of high availability is to survive data center loss
- the high availability can be passive ( for RDS Multi AZ for example )
- the high availability can be active (for horizontal scaling )

# High availability & Scalability for EC2

- Vertical scaling : Increase instances size (=scale up and down)
- Horizontal Scaling : Increase the number of instances you have (=scale out /in )
    - auto scaling Group
    - load balancer
- High availability : runs instance for the same application across multiple AZ
    - auto scaling Group multi AZ
    - Load balancer multi AZ
    
    # Elastic load Balancing
    
- Load balancer are servers that forward traffic to multiple servers ( e.g. EC2) downstream

![Untitled](https://github.com/alinedam/Sitech-Internship/assets/108859223/c6bbc7d8-e9f1-4788-8616-9a10806747a5)


- spread load across multiple downstream instances
- Expose a single point of access (DNS) to your application
- seamlessly handle failures of downstream instances
- do regular health checks to your instances
- provide SSL termination (HTTPS) for your website
- enforce stickiness with cookies
- High availability across zones
- separate public traffic from privet traffic

   

# why use elastic Load Balancing

  

- An Elastic Load Blancer is a managed load blancer
    - AWS guarantees that it will be working
    - AWS takes care of upgrades , maintenance, high availability
    - AWS provides a few configuration knobs
- It costs less to setup your own load balancer but it will be a lot more effort on your ends

 

# Health checks

- Health Checks are crucial for Load Balancers
- they enable the load balancer to know if the instances it  forwards traffic to are available to reply to request  .
- The health check is done on a port and a route ( /health is common )
- if the response is not 200 (OK), then the instance is unhealthy

 ![health check](https://github.com/alinedam/Sitech-Internship/assets/108859223/0233f245-2e7f-4a29-9a32-21d6758adf47)



# Type of load balancer on AWS

- AWS has 4 kinds of managed load balancers
- Classic Load Balancer (V1 bla blaa version ) old version
- Application Load Balancer ( v2 - new generation ) 2016
    - HTTP, HTTPs, WebSocket
- Network Load Balancer ( V2 - new generatoin )
    - TCP.TLS (secrue TCP) ,UDP
- Gateway Load Blancer -2020
    - Operates at layer 3 (Network layer) - IP Protocol
- OVerall , it is recommended to use newer generation load balancer as they provide more feature

 

# Load Balancer Security Groups

Hands on will add it to the repo 

# Network Load Balancer (NLB)

- network load balancer (Layer4) allow to :
    - Forward TCP & UDP traffic to your instance
    - Handle milions of  request per second
    - Less latency ~ 100 ms (vs 400 ms for ALB)
    
- NLB has one static IP per AZ , and supports assining Elastic IP
      (helpful ofr whitelisting specific IP)

- NLB are used for extreme perforamnce , TCP or UDP traffic
- Not included in the AWS free tier

- Network Load Balancer (v2) TCP (Layer 4) Based Traffic
![nlb](https://github.com/alinedam/Sitech-Internship/assets/108859223/005b6b6a-840e-48c5-8583-7e74a2987314)



## NLB- Target Groups

- EC2 instance
- IP Adresses -must be private IPs
- Application Load Balancer
- Health Checks support the TCP,HTTP and HTPTPS Protocols
![nlb target group](https://github.com/alinedam/Sitech-Internship/assets/108859223/f2e5508e-47f1-4395-b09d-7ae224a20479)
# Gateway Load Balancer

- Deploy, scale, and manage a fleet of 3rd party network virtual appliances in AWS
- Example: Firewalls, Intrusion Detection and
Prevention Systems, Deep Packet Inspection
Systems, payload manipulation, …
- Operates at Layer 3 (Network Layer) – IP
Packets
- Combines the following functions:
    - Transparent Network Gateway – single entry/exit for all traffic
    - Load Balancer – distributes traffic to your virtual appliances
    - Uses the GENEVE protocol on port 608
    - 
![Screenshot 2023-08-20 114226](https://github.com/alinedam/Sitech-Internship/assets/108859223/989da27c-d55b-419a-8e25-309ec177f9ee)


# Gateway Load Balancer –Target Groups

- Gateway Load Balancer –Target Groups
- IP Addresses – must be private IPs
- ![Screenshot 2023-08-20 114333](https://github.com/alinedam/Sitech-Internship/assets/108859223/3d50005b-6164-4574-bf15-7f6624c11e89)
