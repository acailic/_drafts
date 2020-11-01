---
title: AWS CDA Practise test 6
layout: post
tags: [aws, cda, test]
date: 2020-08-17
--- 

### Deployment
- Your client wants to deploy a service on EC2 instances, and as EC2 instances are added into an ASG, each EC2 instance should be running 3 different Docker Containers simultaneously.
What Elastic Beanstalk platform should they choose?:Docker multi-container platform. Docker is a container platform that allows you to define your software stack and store it in an image that can be downloaded from a remote repository. Use the Multicontainer Docker platform if you need to run multiple containers on each instance. The Multicontainer Docker platform does not include a proxy server. Elastic Beanstalk uses Amazon Elastic Container Service (Amazon ECS) to coordinate container deployments to multi-container Docker environments.
- You have created a test environment in Elastic Beanstalk and as part of that environment, you have created an RDS database. How can you make sure the database can be explored after the environment is destroyed?:Make a snapshot of the database before it gets deleted. Use an Elastic Beanstalk blue (environment A)/green (environment B) deployment to decouple an RDS DB instance from environment.
### Refactoring
- You are running a public DNS service on an EC2 instance where the DNS name is pointing to the IP address of the instance. You wish to upgrade your DNS service but would like to do it without any downtime. Which of the following options will help you accomplish this?:Route 53 is a DNS managed by AWS, but nothing prevents you from running your own DNS (it's just a software) on an EC2 instance. The trick of this question is that it's about EC2, running some software that needs a fixed IP, and not about Route 53 at all.
Elastic IP. DNS services are identified by a public IP, so you need to use Elastic IP.
- You would like to paginate the results of an S3 List to show 100 results per page to your users and minimize the number of API calls that you will use. Which CLI options should you use? (Select two):--max-items.--starting-token.For commands that can return a large list of items, the AWS Command Line Interface (AWS CLI) has three options to control the number of items included in the output when the AWS CLI calls a service's API to populate the list.--page-size.--max-items.--starting-token. By default, the AWS CLI uses a page size of 1000 and retrieves all available items.
- You are running a web application where users can author blogs and share them with their followers. Most of the workflow is read based, but when a blog is updated, you would like to ensure that the latest data is served to the users (no stale data). The Developer has already suggested using ElastiCache to cope with the read load but has asked you to implement a caching strategy that complies with the requirements of the site.
  Which strategy would you recommend?Use a Write Through strategy.The write-through strategy adds data or updates data in the cache whenever data is written to the database.In a Write Through strategy, any new blog or update to the blog will be written to both the database layer and the caching layer, thus ensuring that the latest data is always served from the cache.
- You are creating a web application in which users can follow each other. Some users will be more popular than others and thus their data will be requested very often. Currently, the user data sits in RDS and it has been recommended by your Developer to use ElastiCache as a caching layer to improve the read performance. The whole dataset of users cannot sit in ElastiCache without incurring tremendous costs and therefore you would like to cache only the most often requested users profiles there. As your website is high traffic, it is accepted to have stale data for users for a while, as long as the stale data is less than a minute old.
 What caching strategy do you recommend implementing?Use a Lazy Loading strategy with TTLLazy loading is a caching strategy that loads data into the cache only when necessary. Whenever your application requests data, it first requests the ElastiCache cache. If the data exists in the cache and is current, ElastiCache returns the data to your application. If the data doesn't exist in the cache or has expired, your application requests the data from your data store. Your datastore then returns the data to your application.
In this case, data that is actively requested by users will be cached in ElastiCache, and thanks to the TTL, we can expire that data after a minute to limit the data staleness.
### Monitoring
- Your client has tasked you with finding a service that would enable you to get cross-account tracing and visualization. Which service do you recommend?: AWS X-Ray.AWS X-Ray is a service that collects data about requests that your application serves and provides tools you can use to view, filter, and gain insights into that data to identify issues and opportunities for optimization. For any traced request to your application, you can see detailed information not only about the request and response but also about calls that your application makes to downstream AWS resources, microservices, databases and HTTP web APIs.
- You have been collecting AWS X-Ray traces across multiple applications and you would now like to index your XRay traces to search and filter through them efficiently.  What should you use in your instrumentation?:Annotations
AWS X-Ray helps developers analyze and debug production, distributed applications, such as those built using a microservices architecture. With X-Ray, you can understand how your application and its underlying services are performing to identify and troubleshoot the root cause of performance issues and errors. X-Ray provides an end-to-end view of requests as they travel through your application, and shows a map of your application’s underlying components. 
- You would like to run the X-Ray daemon for your Docker containers deployed using AWS Fargate.  What do you need to do to ensure the setup will work? (Select two)Deploy the X-Ray daemon agent as a sidecar container. Provide the correct IAM task role to the X-Ray container
### Security
 
### Development with AWS
 