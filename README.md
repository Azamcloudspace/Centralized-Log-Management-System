# Centralized-Log-Management-System

##  Project Summary

Designed and implemented a centralized log ingestion and processing system on AWS to collect, transform, and analyze application logs in real time. The system leverages a streaming architecture to move logs from compute instances into a searchable analytics platform, with durable storage for backup and compliance.

The infrastructure is fully automated using CloudFormation with a modular, nested stack design.

---

##  Key Outcomes

- Built a real-time log ingestion pipeline from EC2 to OpenSearch  
- Implemented log transformation using AWS Lambda  
- Designed a streaming data pipeline using Kinesis Data Firehose  
- Enabled log search and visualization using OpenSearch  
- Implemented durable log backup using Amazon S3  
- Automated infrastructure deployment using CloudFormation  

---

##  Architecture Overview

The system follows a streaming, event-driven pipeline for log processing:

```
EC2 → CloudWatch Logs → Lambda → Firehose → OpenSearch + S3
```

![Architecture](/screenshots/Architecture.jpeg)

---

##  System Design

### Log Generation
- **Amazon EC2**
  - Runs application generating logs (`/var/log/app.log`)  
  - Acts as the primary log source  

---

### Log Collection
- **CloudWatch Agent**
  - Installed on EC2 instance  
  - Streams log data to CloudWatch Logs (`application-logs`)  

---

### Log Processing
- **AWS Lambda**
  - Triggered via CloudWatch Logs subscription filter  
  - Decodes and transforms raw log data  
  - Converts logs into structured JSON format  

---

### Log Streaming
- **Amazon Kinesis Data Firehose**
  - Buffers and delivers processed logs  
  - Ensures reliable delivery to destinations  

---

### Log Storage & Analysis
- **Amazon OpenSearch Service**
  - Indexes logs for real-time querying and visualization  

- **Amazon S3**
  - Stores raw logs for backup and long-term retention  

---

### Security & Configuration
- **AWS IAM**
  - Manages permissions between services  

- **AWS SSM Parameter Store**
  - Stores OpenSearch credentials securely  

---

##  Deployment Architecture

The system is deployed using CloudFormation with nested stacks:

**Deployment Flow:**

```
CodeBuild → CloudFormation → Nested Stacks → Full System Provisioned
```

![DEPLOYMENT](/screenshots/Screenshot1.png)
Screenshot of Codebuild execution

![DEPLOYMENT](/screenshots/Screenshot2.png)
Screenshout of deployed cloudformation stack showing full system provisioned

---

##  Log Processing Lifecycle

1. Application writes logs on EC2  
2. CloudWatch Agent streams logs to CloudWatch  
3. Subscription filter triggers Lambda  
4. Lambda transforms log data  
5. Processed logs sent to Firehose as seen in lambda function code 
6. Firehose delivers logs to:
   - OpenSearch (analysis)  
   - S3 (backup)  

![LIFECYCLE](/screenshots/Screenshot3.png)
Screenshot of Ec2 Server showing App Logs

![LIFECYCLE](/screenshots/Screenshot4.png)
Screenshot of App Logs showing in Clooudwatch

![LIFECYCLE](/screenshots/Screenshot6.png)
Screenshot of Lambda Function with Cloudwatch Subscription filter

![LIFECYCLE](/screenshots/Screenshot8.png)
Screenshot of Firehose Stream with destination; Opensearch. Note: Destination errors where due to opensearch backend roles not being configured at first after deployment

![LIFECYCLE](/screenshots/Screenshot7.png)
Screenshot of OpenSearch Domain with the domain URL

![LIFECYCLE](/screenshots/Screenshot9.png)
Screenshot of App logs being viewed on Opensearch

![LIFECYCLE](/screenshots/Screenshot11.png)
Screenshot of S3 backup logs

---

##  Repository Structure


.
├── ci/
├── cloudformation/
│ ├── child-templates/
│ └── master/

---

##  Automation

- Infrastructure defined using CloudFormation (nested stacks)  
- Deployment automated via CodeBuild  
- Template synchronization via S3  

---

##  Monitoring & Observability

- **CloudWatch Logs**
  - Central log ingestion point  

- **Lambda Logs**
  - Transformation visibility  

- **Firehose Metrics**
  - Delivery success and buffering  

- **OpenSearch Dashboards**
  - Real-time log analysis  

![MONITORING](/screenshots/Screenshot10.png)
Firehose Metrics

![MONITORING](/screenshots/Screenshot12.png)
Opensearch Dashboards


---

##  Security Implementation

- IAM roles with least privilege  
- OpenSearch access restricted via IAM + IP policy  
- Credentials stored securely in SSM  
- Controlled access through service roles  

---

##  DevOps Capabilities Demonstrated

- Log aggregation and centralized monitoring  
- Streaming data pipeline design  
- Event-driven processing  
- Infrastructure as Code (CloudFormation)  
- Real-time analytics system implementation  
- Secure system design  

---

##  Challenges & Resolutions

- **Handling raw log format**  
  Resolved by introducing Lambda transformation layer  

- **Reliable log delivery**  
  Addressed using Kinesis Data Firehose buffering  

- **Access control for OpenSearch**  
  Managed via IAM policies and IP restrictions  

---

##  Future Improvements

- Add CloudWatch alarms for log anomalies  
- Implement structured logging at source
- Introduce observability stack (Prometheus/Grafana)  
- Add alerting and incident response system   

---
