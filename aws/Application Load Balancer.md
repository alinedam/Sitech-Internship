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

# Sticky sessions (session Affinity)

- it’s possible to implement stickiness so that the same client is always redirected to the same instance behind a load balancer
- This works for classic load Balancer , Applicatoin Load Balancer , and Network Load Balancer
- The “cokie” used for stickness  has an expatriation date you control
- Use case: make sure the user doesn’t lose his session data
- Enabling stickiness may bring imbalance to load over the backend EC2 instance

 
![stickiness](https://github.com/alinedam/Sitech-Internship/assets/108859223/43883050-1bc7-4f74-a882-1c0c0e0f74bc)



# sticky session - Cookie Name

- Application- based cookie
    - Custom Cookie
        - generated by the target
        - include any custom attributes required by the application
        - Cookie name must be specific individually for each target group
        - Don’t use AWSALB, AWSALBAPP, or AWSALBTG (reserved for use by the ELB)
    - Application cookie
        - Cookie generated by the load balancer
        - Cookie name is AWSALB for ALB, AWSELB for CLB

# Cross-Zone Load Balancing

![cross zone ](https://github.com/alinedam/Sitech-Internship/assets/108859223/9c82892b-fb33-4bf3-95c4-baa626bb7dec)


# Cross-Zone Load Balancing

- Application Load Balancer
    - Enabled by default (can be disabled at the Target Group level)
    - No charges for inter AZ data
- Network Load Balancer & Gateway Load Balancer
    - Disabled by default
    - You pay charges ($) for inter AZ data if enabled
- Classic Load Balancer
    - Disabled by default
    - No charges for inter AZ data if enabled

# SSL/TLS - Basics

- An SSL Certificate allows traffic between your clients and your load balancer
to be encrypted in transit (in-flight encryption)
- SSL refers to Secure Sockets Layer, used to encrypt connections
- TLS refers to Transport Layer Security, which is a newer version
- Nowadays, TLS certificates are mainly used, but people still refer as SSL

- Public SSL certificates are issued by Certificate Authorities (CA
- Comodo, Symantec, GoDaddy, GlobalSign, Digicert, Letsencrypt, etc…
- SSL certificates have an expiration date (you set) and must be renewed

# Load Balancer -SSL Certificates

![ssl](https://github.com/alinedam/Sitech-Internship/assets/108859223/5c0884a7-9688-4c98-8c95-1c6418623e4a)


- The load balancer uses an X.509 certificate (SSL/TLS server certificate)
- You can manage certificates using ACM (AWS Certificate Manager)
- You can create upload your own certificates alternatively
- HTTPS listener:
    - You must specify a default certificate
    - You can add an optional list of certs to support multiple domains
    - Clients can use SNI (Server Name Indication) to specify the hostname they reach
    - Ability to specify a security policy to support older versions of SSL / TLS (legacy clients)

# SSL – Server Name Indication (SNI)

- SNI solves the problem of loading multiple SSL certificates onto one web server (to serve multiple websites)
- It’s a “newer” protocol, and requires the client
to indicate the hostname of the target server in the initial SSL handshake
- The server will then find the correct
certificate, or return the default one

Note:

- Only works for ALB & NLB (newer
generation), CloudFront
- Does not work for CLB (older gen)

# Elastic Load Balancers – SSL Certificates

- Classic Load Balancer (v1)
    - Support only one SSL certificate
    - Must use multiple CLB for multiple hostname with multiple SSL certificates
- Application Load Balancer (v2)
    - Supports multiple listeners with multiple SSL certificates
    - Uses Server Name Indication (SNI) to make it work
- Network Load Balancer (v2)
    - Supports multiple listeners with multiple SSL certificates
    - Uses Server Name Indication (SNI) to make it work

# Connection Draining

- Feature naming
    - Connection Draining – for CLB
    - Deregistration Delay – for ALB & NLB
- Time to complete “in-flight requests” while the
instance is de-registering or unhealthy
- Stops sending new requests to the EC2
instance which is de-registering
- Between 1 to 3600 seconds (default: 300
seconds)
- Can be disabled (set value to 0)
- Set to a low value if your requests are short
    
![auto scaling 1](https://github.com/alinedam/Sitech-Internship/assets/108859223/10c42463-05e9-480d-82e6-e6aba03eb142)

    

# What’s an Auto Scaling Group?

- In real-life, the load on your websites and application can change
- In the cloud, you can create and get rid of servers very quickly
- The goal of an Auto Scaling Group (ASG) is to:
    - Scale out (add EC2 instances) to match an increased load
    - Scale in (remove EC2 instances) to match a decreased load
    - Ensure we have a minimum and a maximum number of EC2 instances running
    - Automatically register new instances to a load balancer
    - Re-create an EC2 instance in case a previous one is terminated (ex: if unhealthy)
- ASG are free (you only pay for the underlying EC2 instances)

# Auto Scaling Group in AWS

![auto scaling group](https://github.com/alinedam/Sitech-Internship/assets/108859223/31fbe5c3-d888-46a7-9c51-36d402bd9eca)


# Auto Scaling Group in AWS With Load Balancer

![auto sacling with load balancer](https://github.com/alinedam/Sitech-Internship/assets/108859223/617c5ab9-fc72-4f81-b745-a187a8b38445)


# Auto Scaling Group Attributes

- A Launch Template (older “Launch Configurations” are deprecated)
    - AMI + Instance Type
    - EC2 User Data
    - EBS Volumes
    - Security Groups
    - SSH Key Pair
    - IAM Roles for your EC2 Instances
    - Network + Subnets Information
    - Load Balancer Information
- Min Size / Max Size / Initial Capacity
- Scaling Policies

# Auto Scaling - CloudWatch Alarms & Scaling

- It is possible to scale an ASG based on CloudWatch alarms
- An alarm monitors a metric (such as Average CPU, or a custom metric)
- Metrics such as Average CPU are computed for the overall ASG instances
- Based on the alarm:
    - We can create scale-out policies (increase the number of instances)
    - We can create scale-in policies (decrease the number of instances)
![watch alarm ](https://github.com/alinedam/Sitech-Internship/assets/108859223/9ff0bca9-7493-421a-b401-21e74372d1c0)


# Auto Scaling Groups – Dynamic Scaling Policies

- Target Tracking Scaling
    - Most simple and easy to set-up
    - Example: I want the average ASG CPU to stay at around 40%
- Simple / Step Scaling
    - When a CloudWatch alarm is triggered (example CPU > 70%), then add 2 units
    - When a CloudWatch alarm is triggered (example CPU < 30%), then remove 1
- Scheduled Actions
    - Anticipate a scaling based on known usage patterns
    - Example: increase the min capacity to 10 at 5 pm on Fridays
    

# Auto Scaling Groups – Predictive Scaling

- Predictive scaling: continuously forecast load and schedule scaling ahead

# Good metrics to scale on

- CPUUtilization: Average CPUutilization across your instances
- RequestCountPerTarget: to make surethe number of requests per EC2instances is stable
- Average Network In / Out (if you’reapplication is network bound)
- Any custom metric (that you pushusing CloudWatch)

![Good metric](https://github.com/alinedam/Sitech-Internship/assets/108859223/5cebf4fd-24eb-4a56-a905-d749addd7692)


# Auto Scaling Groups - Scaling Cooldowns

- After a scaling activity happens, you are inthe cooldown period (default 300 seconds)
- During the cooldown period, the ASG will
not launch or terminate additionalinstances (to allow for metrics to stabilize)
- Advice: Use a ready-to-use AMI to reduceconfiguration time in order to be serving
request fasters and reduce the cooldown period

![the last one ](https://github.com/alinedam/Sitech-Internship/assets/108859223/376831b4-2c32-4782-8d79-063135a7e0a5)
