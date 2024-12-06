# Must Implemented

1. Route53 vs Cloudflare free DNS
2. DNS --> LB , Should not route to IP directly 
3. LB should have security rules
4. Never use default VPC
5. Any infra or Resources should be in private network including database.
6. There should be at least one VPN instances in every region of the VPC.
7. Traffic always must be forwarded through LB only or VPN , is there no other way.
8. Database backup for project , notification on discord , on success or also if the database is equal or less in size than the Last Day
9. When code merges from dev to staging or staging to production then you should not merge with the commits from dev to staging or staging to dev , You will only merge with one commit that is what you are merging for.
10. RDS , Machine , chat or frontend notification. 
11. Query notification.
12. All media files should me stored on s3 , media files must not be in the project.
13. S3 versioning must be enabled.
14. Every user must have their dedicated creds.
15. database must not be publicly accessible

## Project Architecture 
- monolithic
- modular monolithic
- microservices

# Database Design and Management Guidelines
- **Structured Database Design**
    - Design databases with scalability in mind, ensuring they can handle current application load and future growth.
    - Normalize database schemas where appropriate to minimize redundancy and optimize for query performance.
- **Database Testing and Performance Evaluation**
    - Perform rigorous database testing under expected query loads to evaluate performance and identify bottlenecks.
    - Simulate peak loads to assess system reliability and capacity.
- **Scalability Planning**
    - Develop a database architecture that can be scaled horizontally or vertically as application demands grow.
    - Plan for potential database sharding to distribute data across multiple databases seamlessly if needed in the future.
- **Separation of Read and Write Operations**
    - Implement separate database connections for read and write operations from the start of the project to optimize performance.
    - Use read replicas for scaling read-heavy workloads while maintaining data consistency.
- **Data Lifecycle Management**
    - Define and implement policies for archiving and purging old or infrequently accessed data to maintain optimal performance.
    - Utilize tiered storage for separating frequently accessed data from archival data to reduce costs and improve efficiency.
    - Regularly review data retention policies to comply with legal, regulatory, or organizational requirements.
- **Fault Tolerance and High Availability**
    - Design databases to be fault-tolerant with automatic failover mechanisms to minimize downtime.
    - Implement backup and disaster recovery plans to ensure data safety in case of failures.
- **Monitoring and Metrics Collection**
    - Use monitoring tools to track database performance, query execution times, and resource utilization.
    - Set up alerts for critical thresholds to address issues proactively.
- **Security Best Practices**
    - Enforce secure authentication and access controls for database users.
    - Use encryption for data at rest and in transit to protect sensitive information.
    - Regularly audit and review database permissions and roles.
- **Version Control and Change Management**
    - Maintain version control for database schemas and changes using migration tools.
    - Test all schema changes in a staging environment before applying them to production.
- **Documentation and Standards**
    - Document database schema, relationships, and query patterns for team understanding and consistency.
    - Establish coding standards for database queries to ensure maintainability.
- **Periodic Maintenance**
    - Schedule regular database maintenance tasks, such as indexing, vacuuming, and analyzing table statistics.
    - Archive old or infrequently accessed data to improve performance.

# Code Guidelines
1. **Cron Job Security**
    - Cron jobs must not be publicly accessible. Avoid triggering crons via `curl` or direct URLs to prevent unauthorized access.
    - Use built-in task schedulers (e.g., Laravel Scheduler or Celery) to handle cron jobs securely within the application.
2. **Environment Variables**
    - Environment variables must not be hardcoded or defined directly in the code.
    - Use secure `.env` files or environment management tools for configuration and ensure sensitive data is encrypted or restricted.
3. **Deployment Practices**
    - Avoid using FTP or directly pulling code via Git on the server for deployments.
    - Implement a proper CI/CD pipeline to automate and secure the deployment process.
4. **Branching and Workflow**
    - Developers must work on the **development branch** for active development.
    - To move code between environments:
        - Create a pull request from the **development branch** to the **staging branch** for review and testing.
        - Create another pull request from the **staging branch** to the **production branch** once testing is complete.
    - When starting new features or updates, developers must:
        - Create a **local branch** from the **development branch** for the task.
        - Merge changes into the **development branch** after completing the work.
5. **Database Management**
    - Databases must be created and managed via code (e.g., migration scripts) for consistency and repeatability.
6. **Git Commit Practices**
    - Each merge to the primary branches (development, staging, production) must result in **one merge commit only** to maintain a clean history.
7. **Repository Structure**
    - Every Git repository must include a branch named `vapt.2`.
    - Before merging the **staging branch** into the **production branch**, ensure the **staging branch** is merged into `vapt.2`.
# SMTP
- Configure SMTP credentials securely in environment variables, not in the codebase.
- Use encrypted connections (e.g., TLS or SSL) for email communication to ensure data security.
# Performance Testing , Security & Monitoring


# Pipeline
- For frontend code , build should not be build on local , they must be build on server
- Fastlane should be implemented

---

