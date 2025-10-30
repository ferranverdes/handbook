# 🐜 DevSecOps

## Software Development Life Cycle (SDLC)

| **Phase** | **Description** |
|------------|--------------------------------|
| **1. Planning** | Defines objectives, scope, budget, and resources to ensure project feasibility. |
| **2. Requirements Analysis** | Gathers and documents business, functional, and technical needs of the system. |
| **3. Design** | Translates requirements into detailed system architecture, data structures, and interfaces. |
| **4. Development** | Developers write, review, and integrate code according to the design specifications. |
| **5. Testing** | Executes functional, performance, and security tests to validate system quality. |
| **6. Deployment & Maintenance** | Delivers the system to users, fixes issues, and performs continuous enhancements. |

## DevOps

| **Phase** | **Description** |
|------------|--------------------------------|
| **1. Plan** | Defines features, priorities, and workflows collaboratively between development and operations. |
| **2. Code** | Developers write, version, and review source code using shared repositories. |
| **3. Build** | Code is compiled, dependencies are packaged, and automated builds are created for deployment. |
| **4. Test** | Automated and manual tests validate code functionality, performance, and security. |
| **5. Release** | Approved builds are prepared for production with change management and version control. |
| **6. Deploy** | The application is deployed to staging or production environments using automated pipelines. |
| **7. Operate** | The running system is managed, configured, and supported to ensure stable operation. |
| **8. Monitor** | Continuous observation of logs, metrics, and user feedback ensures reliability and improvement. |

### DevOps Research and Assessment (DORA)

| **DORA Metric** | **Definition** | **Purpose** | **High Performer Benchmark** |
|------------------|----------------|--------------|-------------------------------|
| **Deployment Frequency** | Measures how often new code changes are deployed to production. | Indicates how quickly teams can deliver value and improvements. | Deploy on demand (multiple times per day). |
| **Lead Time for Changes** | Tracks the time from code commit to successful deployment in production. | Reflects development efficiency and automation maturity. | Less than one day. |
| **Change Failure Rate** | Calculates the percentage of deployments that cause service degradation or require a fix. | Evaluates release quality and stability of production deployments. | Less than 15%. |
| **Mean Time to Recovery (MTTR)** | Measures how long it takes to restore normal service after a production failure. | Assesses system resilience and incident response effectiveness. | Less than one hour. |

### CALMS framework

| **Component** | **Definition** | **Purpose** | **Key Practices / Examples** |
|----------------|----------------|--------------|-------------------------------|
| **Culture** | Promotes collaboration, trust, and shared responsibility across teams. | Breaks down silos between Development, Security, and Operations. | Blameless postmortems, cross-functional teams, psychological safety. |
| **Automation** | Uses tools and scripts to automate repetitive tasks throughout the lifecycle. | Increases speed, consistency, and reduces human error. | CI/CD pipelines, Infrastructure as Code (IaC), automated testing. |
| **Lean** | Focuses on eliminating waste and optimising flow for continuous improvement. | Ensures efficient delivery of value with minimal delays. | Value stream mapping, small batch sizes, iterative releases. |
| **Measurement** | Tracks performance, reliability, and process efficiency through data. | Enables data-driven decisions and continuous feedback. | DORA metrics, error rates, MTTR, deployment frequency. |
| **Sharing** | Encourages transparency, communication, and knowledge exchange. | Fosters collective learning and stronger team alignment. | Shared dashboards, internal wikis, open feedback loops. |
