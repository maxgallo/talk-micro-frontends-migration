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

#[fit] Context
## **Migration**
## **Lambda @ Edge**
## **Solution Analysis**

<br />
<br />

### @**_maxgallo**

^ Why that person of that team took a different decision ?

^ Reason for writing down ADR, Architecture Decision Records

---

# Users spikes

![original 40%](./images/users_spike.png)

^ Costs of provisioning machines

^ Warm up not easily manageable with live events, for example delays

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

^ Microservices

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

^ Release one MFE per time with Strangler Pattern is ok, but "catalog" MFE is quite a dangerous one to release globally in one go.

---

# Which Frontend ?

![original 45% ](./images/1_or_2_question.png)

---

# The Solution

[.list: #000000, bullet-character(->), alignment(left)]
[.build-lists: true]

- *Auto Scaling*
- *Fast*
- *Frontend Independent*
- *Same URL*
- *Strangler Pattern*
- *Canary Deployment*

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
An extension to AWS Lambda that lets you execute functions that customize the content that CloudFront delivers.

<br />

- __*No Infrastructure to manage*__
- __*Auto Scaling*__
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

^ The dotted line is where CloudFront chache the items

^ What's the input and the output ? 

---

# Lambda @ Edge
### **CloudFront Events** <br/><br/><br/><br/><br/>

![inline 45%](./images/lambda_at_edge_events.png)


[.footer: Full Specifications: [https://docs.aws.amazon.com/AmazonCloudFront/latest/DeveloperGuide/lambda-event-structure.html](https://docs.aws.amazon.com/AmazonCloudFront/latest/DeveloperGuide/lambda-event-structure.html) ]

---

# Lambda @ Edge Challenges

[.column]
### **Concurrent Limit**
-> 1000 concurrent execution [^a]

### **Deployment Time**
-> up to 10 minutes to deploy


[.column]
### **Metrics & Alarms**
-> Spread across 11 AWS regions

### **Lambda Logs**
-> Spread across 11 AWS regions  [^b]

[^a]: It's possible to increment the limit up to 5000 per account, per region. [https://docs.aws.amazon.com/AmazonCloudFront/latest/DeveloperGuide/cloudfront-limits.html#limits-lambda-at-edge](https://docs.aws.amazon.com/AmazonCloudFront/latest/DeveloperGuide/cloudfront-limits.html#limits-lambda-at-edge)

[^b]: Logs Aggregation [https://aws.amazon.com/blogs/networking-and-content-delivery/aggregating-lambdaedge-logs/](https://aws.amazon.com/blogs/networking-and-content-delivery/aggregating-lambdaedge-logs/)

^ We use Kinesis Firehose to do logs aggregation, which is the method suggested in the blog post.

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

- *Auto Scaling*
- *Fast*
- *Frontend Independent*
- *Same URL*
- **Strangler Pattern**
- **Canary Deployment**

![original 42%](./images/1_or_2_question_final.png)

---

# [fit] Canary Deployments **&** Strangler Pattern

<br/>

![inline 60%](./images/canary_strangler.png)

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

^ We're not working anymore in a Client-Server world, but Client, Server and CDN.

---

#[fit] Thank You


# [fit] **github.com/maxgallo/talk-micro-frontends-migration**

<br />
<br />
<br />

### @**_maxgallo**
