# [fit] Micro-frontends,
### [fit] **a migration story**


<br />
<br />
<br />

### @**_maxgallo**

---

# [fit] Hi 👋 I'm Max

### 🇮🇹 🇬🇧 🍝 💻 🎶 🏍 📷 ✈️ ✍️

### **Principal Engineer @ DAZN**

<br /><br /><br /><br /><br /><br /><br /><br /><br /><br />

### twitter: @**_maxgallo**
### web: **maxgallo.io**

![right 30%](./images/me.png)

---

[.column]
<br /><br /><br /><br /><br /><br /><br />
#[fit] Agenda

[.column]

<br /><br />

## **Context**

## **Migration**

## **Lambda @ Edge**

## **Solution Analysis**

---

<!--
### 1. Contesto


1. DAZN Constraints
2. Soluzione

### 2. Migrazione

1. Due Frontend
2. Routing Missing Part

### 3. Lambda @ Edge
- Overview
- Challenges
	- Development
	- Concurrent Limit
	- Logs & Metrics Aggregation
		- Lambda logs & cloudfront logs
- Tips

### 4. Solution Analysis
- Costs
- Check Needs VS Caracteristic
- Canary Deployment
- Runtime configuration
	-  make network calls to resources in the same region where your Lambda@Edge function is executing to reduce network latency.

### 5. TakeAways

---
-->

#[fit] Context
## **Migration**
## **Lambda @ Edge**
## **Solution Analysis**

<br />
<br />

### @**_maxgallo**

---

# Users spikes

![original 40%](./images/users_spike.png)

^ Costs
^ Warm up not easily manageable

---

# Users spikes
# **-> Scalability**

![original 40%](./images/users_spike.png)

---

# Company Growth

![original 65%](./images/dazn_expansion.png)

^ Example AWS account
^ Before: We didn't know who the credit card assocciated with the account belonged to
^ After: 4 account for each team, everything managed by cli

---

# Company Growth
# **-> Autonomy**

![original 65%](./images/dazn_expansion.png)

---

# Live Events

![original 70%](./images/goal.png)

---


# Live Events
# **-> Speed**

![original 70%](./images/goal.png)


---


[.column]

<br /><br /><br /><br />

# **Users spikes**
# **Company Growth**
# **Live Events**

[.column]

<br /><br /><br /><br /><br /><br /><br /><br />
# [fit] Context


---

<!--
# Holy Trinity of **AWS**

![original 50%](./images/route53_cloudfront_s3.png)

^ Static files
^ Scalability ✓
^ Authonomy ✓
^ Speed ✓

---
-->

## **Context**

#[fit] Migration

## **Lambda @ Edge**
## **Solution Analysis**

<br />

### @**_maxgallo**

---

# Two Frontend Applications, **Two Companies**

![inline 60%](./images/dazn_1_vs_2.png)

---

![original 60% ](./images/monolith_vs_microfrontends.png)

^ Same functionalities

^ Vertical Split of the whole application

^ Domain Driven Design

---

# Micro-frontends

![inline](./images/micro_frontends.png)

[.footer: Micro-frontends resources: [https://medium.com/@lucamezzalira/micro-frontends-resources-53b1ec7d512a](https://medium.com/@lucamezzalira/micro-frontends-resources-53b1ec7d512a)]

^ Independent releases

---

# Migration Challenge 1

## **From Monolith to Micro-Frontends**

---

# Migration Challenge 1

## **From Monolith to Micro-Frontends**

## *-> Strangler Pattern*

![original 43%](./images/strangler_pattern.png)

^ Same URL

---

# Migration Challenge 2

## **Avoid big bang releases**

---

# Migration Challenge 2

## **Avoid big bang releases**

## *-> Canary releases*

![original 43%](./images/canary_release.png)

^ Same URL

---

# Which Frontend ?

![original 45% ](./images/1_or_2_question.png)

---

# The Solution

[.list: #000000, bullet-character(->), alignment(left)]
[.build-lists: true]

- __*Scale Automatically*__
- __*Fast*__
- __*Frontend Independent*__
- __*Same URL*__
- __*Strangler Pattern*__
- __*Canary Deployment*__

![original 40% ](./images/1_or_2_question_long.png)

---

## **Context**

## **Migration**

## [fit] Lambda @ Edge

## **Solution Analysis**

<br />

### @**_maxgallo**

---

# Lambda @ Edge

[.list: #000000, bullet-character(->), alignment(left)]

[.build-lists: true]
An extension to AWS Lambda that lets you execute functions that custmoize the content that CloudFront delivers.

<br />

- __*No Infrastructure to manage*__
- __*Auto Scale*__
- __*Pay-Per-Use*__


![right 60%](./images/lambda.png)

---
<br/>
<br/>
<br/>
<br/>

## Lambda @ Edge
### Run on CloudFront

![original  50%](./images/lambda_at_edge_world.png)

[.footer: 205 Edge Locations and 11 Regional Edge Caches]

---

# Lambda @ Edge

![inline 50%](./images/lambda_at_edge.png)

^ CloudFront events come input & output
^ La riga tratteggiata è dove CloudFront cacha gli oggetti

---

# Lambda @ Edge
### **CloudFront Events** <br/><br/><br/><br/><br/>

![inline 45%](./images/lambda_at_edge_events.png)


[.footer: Full Specifications: [https://docs.aws.amazon.com/AmazonCloudFront/latest/DeveloperGuide/lambda-event-structure.html](https://docs.aws.amazon.com/AmazonCloudFront/latest/DeveloperGuide/lambda-event-structure.html) ]

---


# L@E Challenge #1

## **Concurrent Limit**

1000 concurrent executions per account, per region.

<br/><br/>

Example: *5000 RPS* * *6ms* execution time = *30* concurrent Lambda @ Edge

![inline](./images/lambda.png)![inline](./images/lambda.png)![inline](./images/lambda.png)![inline](./images/lambda.png)![inline](./images/lambda.png)![inline](./images/lambda.png)![inline](./images/lambda.png)![inline](./images/lambda.png)

[.footer: It's possible to increment the limit up to 5000 per account, per region. Lambda @ Edge limits: [https://docs.aws.amazon.com/AmazonCloudFront/latest/DeveloperGuide/cloudfront-limits.html#limits-lambda-at-edge](https://docs.aws.amazon.com/AmazonCloudFront/latest/DeveloperGuide/cloudfront-limits.html#limits-lambda-at-edge)]

---

# L@E Challenge #2

## **Development**

Deploying Lambda@Edge can take up to 10 minutes.

<br/><br/><br/><br/><br/><br/><br/>

![inline 55%](./images/deployment.png)

<br/><br/><br/><br/><br/>

^ AWS said they're improving this one.

---

# L@E Challenge #3

## **Metrics & Alarms**
Available in the region where the Lambda @ Edge did run (11 AWS Regions).

Partially aggregated in CloudFront console.

![inline 30%](./images/users_spike.png)

^ 11 AWS Region sono dove CloudFront ha un Regional Edge Cache

[.footer: Edge monitoring on Cloudfront: [https://aws.amazon.com/about-aws/whats-new/2019/06/announcing-enhanced-lambda-edge-monitoring-amazon-cloudfront-console/](https://aws.amazon.com/about-aws/whats-new/2019/06/announcing-enhanced-lambda-edge-monitoring-amazon-cloudfront-console/)]

---

# L@E Challenge #4

## **Lambda Logs**

CloudWatch logs in the closest region to the request (11 AWS Regions). <br/><br/><br/><br/>

![inline 45%](./images/logs.png)

^ 11 AWS Region sono dove CloudFront ha un Regional Edge Cache

[.footer: Logs Aggregation [https://aws.amazon.com/blogs/networking-and-content-delivery/aggregating-lambdaedge-logs/](https://aws.amazon.com/blogs/networking-and-content-delivery/aggregating-lambdaedge-logs/)]


---

# L@E Challenge #5

## **Lambda Validation Logs**

Lambda @ Edge outputs are validated, and results are available in CloudWatch.

Available at the log group:  __*/aws/cloudfront/LambdaEdge/DistributionId*__


![inline 30%](./images/lambda_at_edge_events.png)

[.footer: Test & Debug Lambda @ Edge: [https://docs.aws.amazon.com/AmazonCloudFront/latest/DeveloperGuide/lambda-edge-testing-debugging.html](https://docs.aws.amazon.com/AmazonCloudFront/latest/DeveloperGuide/lambda-edge-testing-debugging.html) ]
---

## **Context**

## **Migration**

## **Lambda @ Edge**

## [fit] Solution Analysis

<br />

### @**_maxgallo**

---

# Costs

Around __*2ms*__ of execution time

![inline 55%](./images/costs.png)

[.footer: You pay for __*Number of Requests*__ and __*GB per second*__ of memory used (granularity at 50ms) [https://aws.amazon.com/lambda/pricing/#Lambda.40Edge_Pricing](https://aws.amazon.com/lambda/pricing/#Lambda.40Edge_Pricing)]

---

# Lambda @ Edge in DAZN

[.list: #000000, bullet-character(->), alignment(left)]
[.build-lists: true]

- *Auto Scale*
- *Fast*
- *Independent from Frontend*
- *Same URL*
- **Canary Deployment**

![original 42%](./images/1_or_2_question_final.png)

---

# Canary Deployment
### **Two places to store the configuration**

![inline 50%](./images/canary_two_ways.png)

---

# Canary Deployment
### **External Configuration**

![original 40%](./images/canary_external.png)

[.footer: External data in Lambda@Edge: [https://aws.amazon.com/blogs/networking-and-content-delivery/leveraging-external-data-in-lambdaedge/](https://aws.amazon.com/blogs/networking-and-content-delivery/leveraging-external-data-in-lambdaedge/) ]

---

# Canary Deployment
### **Sticky Session**
### **+ Cookies**

![original 40%](./images/lambda_at_edge_sticky.png)

---

# TakeAways

[.list: #000000, bullet-character(->), alignment(left)]
[.build-lists: true]

- Context Context Context !

- Working on the CDN is possibile

- Not only AWS (Cloudflare Workers)

- Routing & Canary Release it's just the beginning (SSR[^1], SEO [^2], Security Headers...)



[^1]: Server Side Rendering

[^2]: Search Engine Optimisation

^ Non lavoriamo più in un contesto di Client-Server, ma di Client, Server & CDN.

---

#[fit] Thank You


# [fit] **github.com/maxgallo/talk-micro-frontends-edge**

<br />
<br />
<br />

### @**_maxgallo**
