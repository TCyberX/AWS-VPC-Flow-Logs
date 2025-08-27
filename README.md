<img src="https://cdn.prod.website-files.com/677c400686e724409a5a7409/6790ad949cf622dc8dcd9fe4_nextwork-logo-leather.svg" alt="NextWork" width="300" />

# VPC Monitoring with Flow Logs

**Project Link:** [View Project](http://learn.nextwork.org/projects/aws-networks-monitoring)

**Author:** Albert  
**Email:** tapcyberx@gmail.com

---

## VPC Monitoring with Flow Logs

![Image](http://learn.nextwork.org/delighted_indigo_timid_orc/uploads/aws-networks-monitoring_3e1e79a1)

---

## Introducing Today's Project!

### What is Amazon VPC?

Amazon VPC (Virtual Private Cloud) and it is useful because provides a secure, isolated, and customizable virtual network in AWS for your cloud resources, allowing to define your own IP address ranges, subnets, and route tables.

### How I used Amazon VPC in this project

In this current project I utilized Amazon VPC to explore and understand in detail how Flow Logs and CloudWatch operate within the traffic of the network, with the goal of improving my ability to troubleshoot issues more effectively in the future.

### One thing I didn't expect in this project was...

I didn’t expect the IAM Roles to policies configuration, or how detailed the data capture is when I run queries in CloudWatch.

### This project took me...

This project took me around to 3h peacefully studing and taking some important notes.

---

## In the first part of my project...

### Step 1 - Set up VPCs

In this step, I will set up a VPC from scratch in minutes. Although monitoring is possible with a single VPC, this project uses two VPCs to reinforce lessons from the VPC peering project.

### Step 2 - Launch EC2 instances

In this step, I will launch two instances: NextWork VPC 1 and NextWork VPC 2, as I plan to test my VPC peering connection later.

### Step 3 - Set up Logs

I will implement a system to track all inbound and outbound network traffic since I’ll be monitoring in a space that stores these records.

### Step 4 - Set IAM permissions for Logs

n this step, I will configure the IAM Policy and Role for VPC Flow Logs to grant permission to write logs and send them to CloudWatch.

---

## Multi-VPC Architecture

I began my project by launching two VPCs and creating two public subnets, without including any private subnets.

The CIDR blocks for VPCs 1 and 2 are 10.1.0.0/16 and 10.2.0.0/16, respectively. They must be unique, as overlapping CIDR blocks can lead to network routing and traffic issues later when traffic needs to flow from one VPC to another.

### I also launched EC2 instances in each subnet

My EC2 instances’ security groups permit SSH and ICMP traffic because EC2 Instance Connect requires SSH-type access, and I need ICMP traffic for connectivity testing later.

![Image](http://learn.nextwork.org/delighted_indigo_timid_orc/uploads/aws-networks-monitoring_e7fa8775)

---

## Logs

Logs are records of your computer system. They capture everything from user logins to errors, serving as the go-to resource for understanding system activity, troubleshooting issues, and monitoring user actions.

Log groups are log data is generated and stored within that particular region. Although the logs are region-specific, you can use CloudWatch dashboards to combine and view logs from different regions in one place.

### I also set up a flow log for VPC 1

![Image](http://learn.nextwork.org/delighted_indigo_timid_orc/uploads/aws-networks-monitoring_e8398869)

---

## IAM Policy and Roles

I created an IAM policy to allow VPC Flow Logs have the permission to record logs and store them in your CloudWatch log group. 
This policy makes sure that your VPC can now send log data to your log group!

I also created an IAM role and assigned it to VPC Logs Flow, granting the permissions in the attached policies.

A custom trust policy is a policy that narrowly defines who can assume a specific role.

![Image](http://learn.nextwork.org/delighted_indigo_timid_orc/uploads/aws-networks-monitoring_4334d777)

---

## In the second part of my project...

### Step 5 - Ping testing and troubleshooting

In this step, I will be generate some traffic because I want to see how VPC 1 send a message to our instance in VPC 2.

### Step 6 - Set up a peering connection

In this step, I will create a peering connections and configure route tables because we have a missing link that's causing this connectivity error between VPCs.

### Step 7 - Analyze flow logs

In this step, I will analyze Flow Logs because has recorded network activity and track insights.

---

## Connectivity troubleshooting

My first ping test between my EC2 instances had no replies, which means there is a problem with the connection between servers.

![Image](http://learn.nextwork.org/delighted_indigo_timid_orc/uploads/aws-networks-monitoring_99d4ba42)

I could receive ping replies if I ran the ping test using the other instance's public IP address, which means I receiving replies from public IPv4 (Instance 2) is correctly configured to respond to ping request.

---

## Connectivity troubleshooting

Looking at VPC 1's route table, I identified that the ping test with Instance 2's private address failed because there's a route to 0.0.0.0/0 in the route table does not get traffic anywhere, including Instance 2.

### To solve this, I set up a peering connection between my VPCs

I updated both VPCs' route tables to direct traffic destined for the other VPC's private IPv4 address through the peering connection instead of the public internet.

![Image](http://learn.nextwork.org/delighted_indigo_timid_orc/uploads/aws-networks-monitoring_7316a13d)

---

## Connectivity troubleshooting

I received ping replies from Instance 2's private IP address! This confirms that setting up the peering connection and updating the route table resolved the VPC-to-VPC connectivity error.

![Image](http://learn.nextwork.org/delighted_indigo_timid_orc/uploads/aws-networks-monitoring_4ec7821f)

---

## Analyzing flow logs

Flow logs indicate the source and destination of traffic, ports, data volume, and whether the transfer was accepted or rejected.

For example, the captured flow log shows traffic from 18.206.107.28 to 10.1.5.222 on port 22, which was accepted.

![Image](http://learn.nextwork.org/delighted_indigo_timid_orc/uploads/aws-networks-monitoring_d116818e)

---

## Logs Insights

Logs Insights is a CloudWatch feature that analyzes your logs; you can view, filter, and combine queries to troubleshoot issues or better understand your network traffic.

I ran the query Top 10 byte transfers by source and destination IP addresses. This query analyzes all about identifying the top 10 largest data transfers between IP addresses in your network, revealing the busiest traffic routes and the resources generating the most data.

![Image](http://learn.nextwork.org/delighted_indigo_timid_orc/uploads/aws-networks-monitoring_3e1e79a1)

---

---
