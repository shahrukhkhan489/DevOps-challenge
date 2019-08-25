# DevOps-challenge

![Diagram](image/Diagram.png?raw=true "Diagram")

### 1. **Briefly describe the conceptual approach you chose! What are the trade-offs?**
Application is deployed in Ubuntu Docker Container using uWSGI Application Server. Strategy for Deployment is as below
- **Cloud Platform** - AWS
- **Container Orchestration** - AWS ECS ( Elastic Contianer Service )
- **Load balancer** - AWS ELB ( Elastic Load Balancer )
- **Scalability** - AWS ECS Service Auto Scaling
- **High Availability** - Can be achieved by leveraging Auto Scaling Group along with Amazon CloudWatch
- **Security** - IAM used for Access Maangement. ECS Containers launched behind VPC Security Group to  control network Security alarms
- **Application/Container Monitoring** - AWS Cloud Watch ( will cost extra as per AWS Pricing Model )
- **Troubleshooting** - Centralize Container Logs by leveraging AWS CloudWatch Logs ( will cost extra as per AWS Pricing Model )

### 2. **What are the operational limits to your approach?**
All containers will be using individual DB instance of SQLite.

### 3. **What will be the annual cost of hosting this solution for 1 year for 10K concurrent users. Describe your method.**
```
For 10,000 Concurrent Users, we will consider the distributions as 
- 1000 Users/Task
- 10 ECS Tasks
- Resources per ECS Task - 4 vCPU and 8GB memory
- 365 days * 24 hours

Yearly CPU charges
Total vCPU charges = # of Tasks x # vCPUs x price per CPU-hour x CPU duration per day (hours) x # of days
Total vCPU charges = 10 x 4 x 0.04256 x 24 x 365 = $14913.024

Yearly memory charges
Total memory charges = # of Tasks x memory in GB x price per GB x memory duration per day (seconds) x # of days
Total memory charges = 10 x 8 x 0.004655 x 24 x 365 = $3262.224

Yearly Fargate compute charges
Yearly Fargate compute charges = Yearly CPU charges + Yearly memory charges
Yearly Fargate compute charges = $14913.024 + $3262.224 = $18175.248
```

### 4. **If you had more time, what improvements would you make, and in what order of priority?**
- Configure Application against AWS RDS(Relational Database Service) Instance by mapping App Config's against Enviornment Variable of Database Instance ( will cost extra as per AWS Pricing Model )
- Complete CI/CD pipeline for Build/Release/Deployment of Application Docker Container using AWS CodeDeploy
