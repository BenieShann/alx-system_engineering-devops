# Postmortem: Outage on E-Commerce Web Application
---

Issue Summary:

### Duration: 
The outage lasted for 2 hours and 45 minutes, starting at 10:15 AM and ending at 1:00 PM (EAT) on August 20, 2024.
Impact: During this period, our e-commerce platform experienced significant slowness, with page load times exceeding 30 seconds, resulting in a complete inability to complete purchases. Approximately 85% of users were affected, with most being unable to add items to their carts or complete transactions.
Root Cause: The root cause was traced to a misconfiguration in our database connection pool, which led to resource exhaustion and subsequent degradation of the application’s performance.

### Timeline:
10:15 AM: We detected the issue through our monitoring system, which triggered an alert due to elevated response times and a spike in error rates.
10:20 AM: Our on-call engineer was notified and began investigating the issue.
10:30 AM: We initially suspected an issue with the web server and investigated the Nginx configuration and logs.
10:45 AM: After ruling out the web server as the cause, we shifted our focus to the application layer.
11:00 AM: We suspected the database might be the cause due to the observed increase in query execution times and escalated the issue to our database team.
11:15 AM: Our database team discovered that the connection pool was exhausted, causing queries to queue up and slow down the entire application.
11:45 AM: We implemented a temporary fix by increasing the connection pool size, which began to stabilize the application.
12:30 PM: We confirmed through monitoring that the application was performing within acceptable parameters.
1:00 PM: We officially declared the incident resolved.

### Root Cause and Resolution:
Root Cause: The issue stemmed from a misconfiguration in our database connection pool. We had configured the pool with a maximum of 100 connections, which was sufficient under normal traffic conditions but inadequate during a traffic surge caused by a promotional campaign. This surge led the connection pool to reach its limit, causing queries to queue up and delaying application responses. The resulting bottleneck caused a significant portion of the application to become unresponsive.

Resolution: We immediately resolved the issue by increasing the connection pool size to 300 connections, which alleviated the queuing problem. We also identified and closed unnecessary connections to free up resources. After restarting the affected services to clear out any lingering issues caused by the resource exhaustion, the application performance returned to normal. We monitored the system closely to confirm stability before declaring the incident resolved.

### Corrective and Preventative Measures:
Improvements and Fixes:

Database Connection Pool Management:

We will adjust the connection pool size dynamically based on traffic patterns to prevent resource exhaustion.
We will implement rate limiting on connections to prevent sudden surges from overwhelming the system.
Monitoring Enhancements:

We will enhance our monitoring to include alerts for connection pool usage, allowing us to detect potential issues earlier.
We will implement more granular monitoring of application performance metrics to identify bottlenecks more quickly.
Load Testing and Capacity Planning:

We will conduct regular load testing, especially before promotional events, to ensure the system can handle increased traffic.
We will review and update our capacity planning strategies to better align with expected user traffic and application demands.

### Tasks:
We will patch the database configuration to allow dynamic adjustment of the connection pool size.
We will update our monitoring tools to include alerts for connection pool thresholds and application performance metrics.
We will schedule load testing sessions before each major promotional event.
We will conduct a review of our current capacity planning process and adjust it to better account for traffic surges.

### A Little Humor to Brighten Your Day:
Why did the database connection refuse to work during the traffic surge?
Because it couldn’t handle the pressure and needed more “connections” in its life!

By implementing these corrective and preventative measures, we aim to prevent a recurrence of this issue and improve the overall reliability of our system during high-traffic events—so our users can shop without feeling like they’re stuck on dial-up.
