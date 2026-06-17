# Databricks Q&A

---

**1. Describe the different cluster modes available in Databricks**

There are 2 main compute modes available in Databricks:

1. **Shared/interactive compute (All-purpose cluster):**
   - These clusters are created manually by the user.
   - They can be shared between multiple users and are used for interactive workloads.
   - Since they may stay active for longer durations and support multiple users, they can be more expensive than a job cluster.
   - Common use cases:
     - notebook development
     - collaborative work
     - data exploration and analysis

2. **Normal/automated compute (Job cluster):**
   - This cluster is created to run a specific job, which may contain one or more tasks.
   - It is automatically created when the job starts.
   - The job cluster terminates automatically when the job completes. This makes it more cost-efficient than shared compute, and each run gets a fresh, isolated environment.

---

**2. Explain the concept of cluster autoscaling in Databricks.**

In Databricks there is an additional option when we are creating new cluster, where user can select this autoscaling by clicking on the checkbox.

What its working is Databricks automatically adjust the number of worker nodes in a cluster based on the workload requirements.

e.g. we can suppose a cluster is configured with:
- Minimum workers = 2
- Maximum workers = 10

If the workload is small, then the cluster may run with 2 workers. As the workload inc. the Databricks can automatically add more workers but up to 10. Once the workload dec. it will reduce the number of workers back down to 2 workers.

---

**3. What are the advantages of using Delta Lake over Parquet or ORC formats?**

Firstly Parquet or ORC formats are used for storing data efficiently, where Delta Lake is a storage layer but built on top of the formats which was mentioned, additionally it provide data management, reliability

But Main advantages would be:

1. **ACID Transaction**
   - ACID (Atomicity, Consistency, Isolation, Durability)
2. **Time Travel**
   - We can access the previous versions of the table (could restore a table to its prev. state)
3. **Data Versioning**
   - Every change we make to Delta Table is tracked in the transaction log (complete history saved).

---

**4. How would you handle large dataset that do not fit into memory? Discuss both storage and processing considerations.**

To handle such large dataset, I would focus on both storage optimizations and apply distributed processing techniques

- **Storage Consideration**
  - Use Columar storage formats i.e Parquet or Delta Late instead of csv or json
  - Partition the data based on frequently filtered columns i.e date, region
- **Processing Consideration**
  - Use distributed processing such as Apache Spark to compute the data in multiple worker nodes parallelly instead of single machine memory.

---

**5. Provide a scenario where Delta Lake's time travel feature would be particularly useful.**

If we accidentally ran any data modification or deletion. In such cases, if we run an incorrect UPDATE, DELETE statement on Delta table which contains customer data, which results in modifying 100's or 1000's of records

To recover that we can use this:

```sql
SELECT * FROM customer_details VERSION AS OF 10;   -- view the prev. data
SELECT * FROM customer_details TO VERSION AS OF 10; -- restore the prev. data
```

---

**6. How would you ensure compliance with data privacy regulations using Databricks?**

To ensure compliance I would implement data encryption, use Unity Catalog for Data Governance and use role-based access control (RBAC). This can further be improved by using them in combination.

---

**7. Explain the difference between a Job Cluster and an All-Purpose Cluster in Databricks. Under what circumstances would you choose one over the other?**

**Shared/interactive compute (All-purpose cluster):**
- These clusters are created manually by the user.
- They can be shared between multiple users and are used for interactive workloads.
- Since they may stay active for longer durations and support multiple users, they can be more expensive than a job cluster.
- Common use cases:
  - notebook development
  - collaborative work
  - data exploration and analysis

**Normal/automated compute (Job cluster):**
- This cluster is created to run a specific job, which may contain one or more tasks.
- It is automatically created when the job starts.
- The job cluster terminates automatically when the job completes. This makes it more cost-efficient than shared compute, and each run gets a fresh, isolated environment.
- Common use cases:
  - ETL pipeline
  - scheduled workflows

In most cases, use shared compute for interactive development and collaborative analysis, and use normal compute for production jobs and scheduled ETL runs.

---

**8. What are the advantages and disadvantages of Serverless clusters, and in what scenarios are they most effectively used?**

- **Advantages:**
  - No cluster management, no setup required
  - Faster startup time
  - Automatic scaling on workload
- **Disadvantages:**
  - Less control over the cluster configurations.
  - Its availability depends on Databricks cloud provider.
  - Not suitable for workload which requires specific infrastructure settings.
- **Its mostly effective on:**
  - Interactive notebook workload.
  - Data exploration by analysts and data scientists.

---

**9. How does Databricks handle version control for notebooks and other code artifacts? Discuss the available options and best practices.**

Databricks support version control through Git Integration and workspace revision history.

Available options:
- **Git Integration:** connect external repositories to Databricks i.e. GitHub, GitLab.
- **Databricks Repos:** Allows dev to clone, pull, commit and push code changes directly to databricks
- **Notebook Revision History:** track changes made on notebook & allows to view and restore.

Best Practices:
- Use feature branch for development.
- Follow code review and pull request process.
- Commit changes frequently with meaningful commit messages.
- Avoid storing secrets in notebooks.

---

**10. Describe the role of Databricks Repos in Collaborative development. How do they differ from traditional Git repositories?**

Primary role of Databricks Repos in Collaborative development is in the following:
- Its basically a Git based workflow within Databricks
- Dev's can collaborate to clone, create branches, commit changes and solve merge conflicts.
- Improves collaboration, version control and code sharing among team members

Differ from traditional Git repositories:
- Databricks repo workspace is managed by Databrick themselves
- Traditional Git repo are hosted and managed by GitHub, GitLab or Azure DevOps

---

**11. What are Databricks Widgets, and how can they be used to make notebooks more interactive? Provide an example use case.**

It's a simple input control that allows users to pass parameters into notebook without modifying the code. Using a inbuilt dbutils utility.

Provides common widget type in (dbutils.widget):
- "Text"
- "Dropdown"

---

**12. Discuss the importance of monitoring and alerting in Databricks. What tools and features are available for tracking performance and identifying issues?**

Importance:
- It helps identify any failure and bottleneck.
- Ensure job successfully completed.
- Reducing downtown by detecting issues early.

Tools and Features Available:
- **Job Monitoring:** Track job status, execution time, and failure.
- **Audit Logs:** Track user activities and system events.

---

**13. What is Databricks Assistant and how can it help you?**

Its an AI Assistant called Genie Code, helps in code completion, review and reading multiple files for proper context. Additionally, it can edit files and do the cell wise investigation for any syntax error, compile error.

---

**14. How does the Databricks Assistant learn or adapt to user's workspace context?**

Databricks Assistant is context-aware and can understand the current notebook, code, queries, schemas, and workspace object that user is working with. Additionally it uses the retrieval-based techniques to incorporate relevant workspace information into the model's prompt. It maintains conversation history and workspace context so follow-up question can be understood and answered.

---

**15. Compare Databricks Assistant and GitHub Copilot?**

- **Databrick Assistant**
  - Built specificly for the Databricks platform.
  - Integrated within Databricks notebooks and workspace.
  - Understands and helps in spark, sql, etc languages and notebooks contexts with spark jobs.
- **GitHub Copilot**
  - General-purpose AI coding assistant
  - Primarily understands the code in the current file/project
  - Helps in code generation across multiple programming languages.

---

**16. What is the purpose of MLflow in the Databricks environment?**

Its an open-source platform used to help manage the end-to-end machine learning lifecycle. It help data scientists keep track of their machine learning experiment and models.

---

**17. Describe one way that Databricks simplifies machine learning workflows.**

Databricks simplifies machine learning workflow through its inbuilt integration with MLflow. MLflow help users to keep tracks of its model's experiment, performance, manage models versions and deploy model from a single platform. Additionally, makes whole process organized and efficient.

---

**18. What is the Databricks Feature Store and why is it useful?**

It's a centralized repository where machine learning features can be store, manage and share. Instead of every data scientist creating the same features again and again, they can simply reuse features that already exist in the Feature Store.

---

**19. How can a user register and deploy a model in Databricks?**

**Register a Model:**
- Train the model using MLflow.
- Register the modle in the MLflow Model Registry.
- Assign versions and manage model

**Deploy a Model:**
- Select the registered model version.
- Move it to the needed stage.
- Deploy it as a serving endpoint for real-time inference or use it in jobs.

---

**20. What is one key advantage of using notebooks in Databricks for ML development?**

One key advantage of using Databricks notebooks for machine learning development is that they provide an interactive and collaborative environment where users can perform data exploration, model development, visualization in a single place. This helps accelerate the development and improve collaboration among team members.
