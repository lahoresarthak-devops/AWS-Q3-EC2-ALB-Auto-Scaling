# AWS Q3 – EC2 + Application Load Balancer + Auto Scaling

## Project Overview

This project demonstrates a highly available web application deployed on AWS using Amazon EC2, Application Load Balancer (ALB), Auto Scaling Group (ASG), Target Group, Amazon VPC, and Security Groups.

The application is deployed across two Availability Zones to improve availability and provide automatic instance management.

## Architecture

```text
                         Internet
                            |
                            v
              +---------------------------+
              | Application Load Balancer |
              |        HTTP : 80          |
              +-------------+-------------+
                            |
                            v
                       Target Group
                       HTTP : 80
                        /      \
                       /        \
                      v          v
               ap-south-1a  ap-south-1b
                    |            |
                    v            v
                 EC2            EC2
                    \            /
                     \          /
                  Auto Scaling Group
```

## AWS Configuration

| Component | Configuration |
|---|---|
| Region | ap-south-1 (Mumbai) |
| Auto Scaling Group | `q3-web-asg` |
| Launch Template | `q3-web-app-template` |
| Instance Type | `t3.micro` |
| Desired Capacity | 2 |
| Minimum Capacity | 2 |
| Maximum Capacity | 4 |
| Availability Zones | ap-south-1a, ap-south-1b |
| Target Group | `q3-web-targets` |
| Target Type | Instance |
| Protocol | HTTP |
| Port | 80 |
| Load Balancer | `q3-web-alb` |
| Health Check | HTTP |

## 1. Launch Template

The launch template `q3-web-app-template` is used by the Auto Scaling Group to launch the EC2 web servers. The instances use the `t3.micro` instance type and the configured security group allows HTTP traffic on port 80.

## 2. Auto Scaling Group

The Auto Scaling Group `q3-web-asg` is configured with:

- Minimum capacity: 2
- Desired capacity: 2
- Maximum capacity: 4
- Instances across two Availability Zones
- Launch template: `q3-web-app-template`

The final setup shows two healthy instances at the desired capacity.

## 3. EC2 Instances

Two `t3.micro` EC2 instances are managed by the Auto Scaling Group. Both instances are **InService** and **Healthy**, with instances distributed across two Availability Zones.

## 4. Target Group

The target group `q3-web-targets` is configured with:

- Target type: Instances
- Protocol: HTTP
- Port: 80
- Total targets: 2
- Healthy targets: 2
- Unhealthy targets: 0

The target group is associated with the Application Load Balancer `q3-web-alb`.

## 5. Application Load Balancer

The Application Load Balancer `q3-web-alb` receives HTTP requests and forwards them to healthy EC2 instances through the target group.

## 6. Health Checks

The target group performs HTTP health checks on the registered EC2 instances. Only healthy targets receive traffic from the Application Load Balancer.

Current health result:

- 2 total targets
- 2 healthy
- 0 unhealthy

## 7. Application Testing

The web application was successfully accessed through the load-balanced setup. The application page displays:

**Q3 AWS Auto Scaling Web Application**

This confirms that the web application is running and accessible through the load-balanced architecture.

## 8. Availability and Recovery

The application is deployed across two Availability Zones. If one EC2 instance becomes unavailable, the target group health check can detect the unhealthy target, the ALB can stop sending traffic to it, and the Auto Scaling Group can launch a replacement instance to maintain the configured capacity.

## Screenshots / Evidence

### 1. ALB Application Test

![ALB Application Test](screenshots/01-alb-application-test.png)

### 2. Auto Scaling Group

![Auto Scaling Group](screenshots/02-auto-scaling-group.png)

Shows `q3-web-asg` with 2 instances, 2/2 healthy, and desired capacity of 2.

### 3. EC2 Instance Management

![EC2 Instance Management](screenshots/03-ec2-instance-management.png)

Shows two EC2 instances as InService and Healthy across two Availability Zones.

### 4. Target Group Health

![Target Group Health](screenshots/04-target-group-healthy.png)

Shows 2 total targets, 2 healthy targets, and 0 unhealthy targets, with `q3-web-alb` attached.

## Testing Result

The final AWS environment demonstrates:

- EC2 web application deployment
- Two EC2 instances across multiple Availability Zones
- Auto Scaling Group with desired capacity of 2
- Application Load Balancer
- Target Group with healthy targets
- HTTP health checks
- Successful application access through the ALB

## Conclusion

This project demonstrates a highly available AWS web application using EC2, Application Load Balancer, Target Group, and Auto Scaling Group. The architecture provides traffic distribution, health monitoring, multi-AZ deployment, and automatic instance management.

## Folder Structure

```text
AWS-Q3-EC2-ALB-Auto-Scaling/
├── README.md
└── screenshots/
    ├── 01-alb-application-test.png
    ├── 02-auto-scaling-group.png
    ├── 03-ec2-instance-management.png
    └── 04-target-group-healthy.png
```
