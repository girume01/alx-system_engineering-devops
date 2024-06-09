Incident Postmortem: Web Application Outage
Issue Summary
Duration: April 5, 2024, from 2:00 PM to 4:30 PM (PDT)
Impact: The web application was down, affecting 80% of users who experienced complete service unavailability. Users were unable to access the site, resulting in a significant loss of service and user frustration.
Root Cause: A misconfigured database query caused a deadlock, leading to the exhaustion of database connections and subsequent application failure.


Timeline
2:00 PM: Issue detected via monitoring alert indicating high database response times.
2:05 PM: Engineers began investigating the database server for performance issues.
2:15 PM: Initial assumption was high traffic load; scaled up the database instance to handle more connections.
2:30 PM: Escalated to the database team after no improvement in performance.
2:45 PM: Database team identified deadlocks as the potential issue, restarted the database server to temporarily restore service.
3:00 PM: Further investigation into recent code deployments to identify changes in database queries.
3:15 PM: Misleading path: Assumed a network issue between the application server and the database server, checked network logs.
3:30 PM: Identified a recently deployed feature with a complex query that was causing deadlocks.
3:45 PM: Rolled back the recent deployment and restarted the application servers.
4:00 PM: Service partially restored, continued monitoring for stability.
4:30 PM: Full service restoration confirmed.


Root Cause and Resolution
Root Cause: The root cause was a complex SQL query introduced in a recent deployment. The query involved multiple joins and subqueries, which led to database deadlocks. As these deadlocks accumulated, all available database connections were exhausted, causing the web application to fail.
Resolution: The resolution involved rolling back the recent deployment that introduced the problematic query. Once the deployment was rolled back, the application and database servers were restarted to clear the existing deadlocks and restore normal operations. Subsequently, the problematic query was optimized by breaking it down into simpler queries and ensuring proper indexing on the involved tables.


Corrective and Preventative Measures
Improvements:
Implement pre-deployment query performance testing to catch inefficient queries before they reach production.
Enhance monitoring to detect and alert on database deadlocks and connection pool exhaustion.
Conduct regular code reviews focusing on database query optimization and performance impacts.
Improve the rollback procedures to ensure quicker recovery in future incidents.


Tasks:
Patch Nginx server: Ensure the web server is capable of handling increased loads and has optimized configuration settings.
Add monitoring on server memory: Implement additional monitoring to track memory usage on both application and database servers.
Optimize problematic query: Refactor the identified query into multiple simpler queries and add necessary indexing.
Develop pre-deployment testing: Create automated tests that simulate production traffic and check for query performance issues.
Train team on database performance: Conduct training sessions for the development team on best practices for writing efficient SQL queries.
Review and update incident response plan: Update the incident response plan to include steps for handling database-specific issues.

