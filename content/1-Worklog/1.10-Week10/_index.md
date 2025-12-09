---
title: "Week 10 Worklog"
date: "2025-11-16"
weight: 10
chapter: false
pre: " <b> 1.10. </b> "
---



### Week 10 Objectives:

* Focus on enhancing and optimizing the performance of the weather API.
* Attend the AWS Cloud Mastery Series #1 event.
* Implement improvements to make the API more robust and efficient.

### Tasks to be carried out this week:
| Day | Task                                                                                                                                                                                                   | Start Date | Completion Date | Reference Material                        |
| --- | ------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------ | ---------- | --------------- | ----------------------------------------- |
| 2 | **Implement API Caching:** <br> - Research and implement caching strategies using Amazon ElastiCache (Redis) or API Gateway caching. <br> - Apply caching for frequently requested weather data (e.g., by city) to reduce external API calls and latency. | 10/11/2025 | 10/11/2025 | 
| 3 | **Error Handling & Retry Logic:** <br> - Improve error handling in Lambda functions for external API failures. <br> - Implement retry logic with exponential backoff for transient errors. <br> - Define clear error response formats for the API. | 11/11/2025 | 11/11/2025 |
| 4 | **Optimize Lambda Performance:** <br> - Review and adjust Lambda function configurations (memory, timeout). <br> - Implement Lambda layers for shared dependencies (e.g., weather API client). <br> - Optimize code for cold start reduction. | 12/11/2025 | 12/11/2025 |
| 5 | **API Monitoring & Logging:** <br> - Set up Amazon CloudWatch Logs for Lambda function logging. <br> - Create CloudWatch Alarms for error rates and high latency. <br> - Implement structured logging for easier debugging. | 13/11/2025 | 13/11/2025 | CloudWatch |
| 6 | **Security & Rate Limiting:** <br> - Configure API Gateway usage plans and API keys for basic rate limiting. <br> - Review and tighten IAM roles and policies for Lambda functions. <br> - Ensure secure handling of external API keys using AWS Secrets Manager. | 14/11/2025 | 14/11/2025 | API Gateway, IAM, Secrets Manager |
| 7 | **Attend AWS Cloud Mastery Series #1:** <br> - Participate in the event to learn about advanced AWS services and architectures. <br> - Document key takeaways relevant to the project. | 15/11/2025 | 15/11/2025 | 

### Week 10 Achievements:

* Enhanced API robustness with improved error handling and retry mechanisms.
* Optimized Lambda functions for better performance and manageability.
* Established monitoring and logging for operational visibility.
* Improved API security with rate limiting and secure credential management.
* Gained new insights from the AWS Cloud Mastery Series event.