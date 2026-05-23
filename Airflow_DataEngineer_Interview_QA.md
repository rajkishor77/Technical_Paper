# 🌊 Apache Airflow Interview Questions & Answers
### JSW Data Engineer Interview Preparation — Friday

---

## 📋 Table of Contents
> Click on any question to jump directly to its answer

- [Q1. What is Apache Airflow?](#q1)
- [Q2. What are the Core Components of Airflow?](#q2)
- [Q3. What is a DAG in Airflow?](#q3)
- [Q4. What are Tasks and Operators in Airflow?](#q4)
- [Q5. What are the different types of Operators in Airflow?](#q5)
- [Q6. What is a Sensor in Airflow?](#q6)
- [Q7. What is the difference between Operator and Sensor?](#q7)
- [Q8. What are the different Executors in Airflow?](#q8)
- [Q9. What is the Airflow Scheduler?](#q9)
- [Q10. What are the different Task States in Airflow?](#q10)
- [Q11. What is XCom in Airflow?](#q11)
- [Q12. What is the TaskFlow API in Airflow 2.0?](#q12)
- [Q13. What is the difference between XCom and Variables?](#q13)
- [Q14. What are Airflow Hooks?](#q14)
- [Q15. What is the difference between Hooks and Operators?](#q15)
- [Q16. How do you define Task Dependencies in Airflow?](#q16)
- [Q17. What is a Trigger Rule in Airflow?](#q17)
- [Q18. What are Airflow Connections?](#q18)
- [Q19. What are Airflow Variables?](#q19)
- [Q20. What is Jinja Templating in Airflow?](#q20)
- [Q21. What is the catchup parameter in Airflow?](#q21)
- [Q22. What is Backfilling in Airflow?](#q22)
- [Q23. What is the execution_date in Airflow?](#q23)
- [Q24. What is a SubDAG and TaskGroup?](#q24)
- [Q25. What is BranchPythonOperator in Airflow?](#q25)
- [Q26. What are Airflow Pools?](#q26)
- [Q27. What are Airflow Queues?](#q27)
- [Q28. How does Airflow handle Retries?](#q28)
- [Q29. What is DAG Serialization in Airflow?](#q29)
- [Q30. What is the Airflow Metadata Database?](#q30)
- [Q31. What is Dynamic DAG Generation?](#q31)
- [Q32. What is Airflow's CeleryExecutor and how does it work?](#q32)
- [Q33. What is KubernetesExecutor in Airflow?](#q33)
- [Q34. What is the difference between CeleryExecutor and KubernetesExecutor?](#q34)
- [Q35. How do you make a DAG Idempotent?](#q35)
- [Q36. How do you handle failures and alerts in Airflow?](#q36)
- [Q37. What is an Airflow DAG Run?](#q37)
- [Q38. What is the difference between schedule_interval and timetable?](#q38)
- [Q39. How do you pass parameters to a DAG at runtime?](#q39)
- [Q40. What are Airflow Best Practices for Data Engineers?](#q40)
- [Q41. What is the difference between Airflow and other orchestrators (Luigi, Prefect, Dagster)?](#q41)
- [Q42. How do you deploy Airflow in production?](#q42)
- [Q43. How does Airflow integrate with GCP BigQuery?](#q43)
- [Q44. How does Airflow integrate with Apache Kafka?](#q44)
- [Q45. What is a real-world ETL pipeline using Airflow?](#q45)

---

## 🔵 SECTION 1: AIRFLOW BASICS (Q1–Q10)

---

<a id="q1"></a>
### 🔷 Q1. What is Apache Airflow?

> **Definition:**
> - Apache Airflow is an open-source platform to programmatically author, schedule and monitor data pipelines (workflows)
> - Created at Airbnb in October 2014, later donated to Apache Software Foundation in 2016
> - Written in Python — pipelines are defined as Python code
> - As of 2025, Airflow is the de facto tool for data engineering and has been adopted by Fortune 500 companies
>
> **What Airflow does:**
> - Defines workflows as code — called DAGs (Directed Acyclic Graphs)
> - Schedules workflows to run automatically at specified times
> - Monitors execution — shows which tasks succeeded, failed or are running
> - Handles retries automatically on task failure
> - Sends alerts when pipelines fail
>
> **Why Data Engineers use Airflow:**
> - Replace cron jobs — much more powerful and manageable
> - Manage complex pipeline dependencies — Task A must finish before Task B
> - Visualize pipeline flow in UI — easy debugging
> - Retry failed tasks automatically
> - Integrate with 100+ services — databases, cloud, APIs
> - Track pipeline history and audit logs
>
> **Common Use Cases:**
> - ETL pipelines — extract from source, transform, load to warehouse
> - Data validation — run quality checks after loading data
> - ML pipelines — train model, evaluate, deploy
> - Report generation — generate and email daily reports
> - Database sync — replicate data between databases

---

<a id="q2"></a>
### 🔷 Q2. What are the Core Components of Airflow?

> **Airflow has 4 core components:**
>
> **1. Scheduler:**
> - Brain of Airflow — monitors all DAGs and tasks continuously
> - Triggers tasks when their dependencies are met
> - Works with cron expressions or custom intervals
> - Sends tasks to the Executor for actual execution
> - Runs as a separate process
>
> **2. Executor:**
> - Determines HOW and WHERE tasks are executed
> - Types: SequentialExecutor, LocalExecutor, CeleryExecutor, KubernetesExecutor
> - SequentialExecutor — runs one task at a time (development only)
> - CeleryExecutor — distributes tasks across multiple worker machines
> - KubernetesExecutor — each task runs in its own Kubernetes pod
>
> **3. Web Server (UI):**
> - Built on Flask — runs as a web application
> - Visual interface to monitor DAGs, tasks and pipeline runs
> - Shows task logs, execution history, task status
> - Allows manually triggering DAGs
> - Manage connections, variables and pools
>
> **4. Metadata Database:**
> - Stores all Airflow state — DAG runs, task states, connections, variables, XComs
> - Default: SQLite (dev only) | Production: PostgreSQL or MySQL
> - Scheduler reads from here to know what to run next
> - Every task status update is written here
>
> **Additional Components:**
> - **Workers** — processes that actually execute tasks (used with CeleryExecutor)
> - **Message Broker** — Redis or RabbitMQ (used with CeleryExecutor)
> - **DAGs Folder** — directory where Python DAG files are stored

---

<a id="q3"></a>
### 🔷 Q3. What is a DAG in Airflow?

> **DAG = Directed Acyclic Graph**
> - A DAG is a collection of tasks organized with dependencies defining the order of execution
> - **Directed** — tasks have a specific direction (A → B → C)
> - **Acyclic** — no circular dependencies allowed (A cannot depend on C if C depends on A)
> - **Graph** — tasks are nodes, dependencies are edges
>
> **Key DAG properties:**
> - `dag_id` — unique name for the DAG
> - `schedule_interval` — how often to run (cron expression or preset)
> - `start_date` — when the DAG should start running
> - `catchup` — whether to run missed past runs
> - `default_args` — default settings for all tasks (owner, retries, etc.)
>
> ```python
> from airflow import DAG
> from airflow.operators.python import PythonOperator
> from datetime import datetime, timedelta
>
> # Default arguments for all tasks
> default_args = {
>     'owner': 'rajkishor',
>     'retries': 3,
>     'retry_delay': timedelta(minutes=5),
>     'email_on_failure': True,
>     'email': ['rajkishor@example.com']
> }
>
> # Define the DAG
> with DAG(
>     dag_id='daily_sales_pipeline',
>     default_args=default_args,
>     start_date=datetime(2024, 1, 1),
>     schedule_interval='0 2 * * *',  # Run daily at 2 AM
>     catchup=False,
>     description='Daily sales ETL pipeline'
> ) as dag:
>
>     def extract():
>         print("Extracting data...")
>
>     def transform():
>         print("Transforming data...")
>
>     def load():
>         print("Loading data...")
>
>     extract_task = PythonOperator(task_id='extract', python_callable=extract)
>     transform_task = PythonOperator(task_id='transform', python_callable=transform)
>     load_task = PythonOperator(task_id='load', python_callable=load)
>
>     # Define order: extract → transform → load
>     extract_task >> transform_task >> load_task
> ```

---

<a id="q4"></a>
### 🔷 Q4. What are Tasks and Operators in Airflow?

> **Task:**
> - A single unit of work inside a DAG
> - When an Operator is added to a DAG, it becomes a Task
> - Each task has a unique `task_id` within the DAG
> - Tasks are the nodes in the DAG graph
>
> **Operator:**
> - A template that defines what a task does
> - Operators are classes — you instantiate them to create tasks
> - Think of it as: Operator = what to do, Task = the actual instance running
>
> **Task lifecycle:**
> ```
> none → scheduled → queued → running → success
>                                     → failed → up_for_retry → running
>                                     → skipped
> ```
>
> **Relationship:**
> ```python
> # PythonOperator is the template
> # extract_task is the Task instance
> extract_task = PythonOperator(
>     task_id='extract_data',
>     python_callable=my_extract_function,
>     dag=dag
> )
> ```

---

<a id="q5"></a>
### 🔷 Q5. What are the different types of Operators in Airflow?

> **1. Action Operators — perform an action:**
> - `PythonOperator` — runs a Python function
> - `BashOperator` — runs a bash command or script
> - `EmailOperator` — sends an email
> - `DummyOperator` / `EmptyOperator` — does nothing, used as placeholder
>
> **2. Transfer Operators — move data:**
> - `S3ToGCSOperator` — copy data from AWS S3 to GCS
> - `GCSToBigQueryOperator` — load GCS file to BigQuery
> - `PostgresToGCSOperator` — export PostgreSQL to GCS
>
> **3. Cloud Operators — interact with cloud services:**
> - `BigQueryInsertJobOperator` — run BigQuery SQL query
> - `BigQueryCheckOperator` — validate BigQuery query returns results
> - `GCSDeleteObjectsOperator` — delete files from GCS
> - `DataflowCreatePythonJobOperator` — run a Dataflow job
>
> **4. Database Operators:**
> - `PostgresOperator` — run SQL on PostgreSQL
> - `MySqlOperator` — run SQL on MySQL
> - `SqliteOperator` — run SQL on SQLite
>
> ```python
> from airflow.operators.python import PythonOperator
> from airflow.operators.bash import BashOperator
> from airflow.providers.google.cloud.operators.bigquery import BigQueryInsertJobOperator
>
> # Python Operator
> run_python = PythonOperator(
>     task_id='run_script',
>     python_callable=my_function
> )
>
> # Bash Operator
> run_bash = BashOperator(
>     task_id='run_bash',
>     bash_command='echo "Pipeline started" && date'
> )
>
> # BigQuery Operator
> run_bq_query = BigQueryInsertJobOperator(
>     task_id='run_bq',
>     configuration={
>         "query": {
>             "query": "SELECT COUNT(*) FROM dataset.orders",
>             "useLegacySql": False
>         }
>     }
> )
> ```

---

<a id="q6"></a>
### 🔷 Q6. What is a Sensor in Airflow?

> **Definition:**
> - A Sensor is a special type of Operator that waits for a specific condition to be true before proceeding
> - It keeps checking (polling) until the condition is met or it times out
>
> **Common Sensors:**
> - `FileSensor` — waits for a file to appear at a path
> - `S3KeySensor` — waits for a file in AWS S3
> - `GCSObjectExistenceSensor` — waits for a file in GCS
> - `HttpSensor` — waits for an HTTP endpoint to return success
> - `SqlSensor` — waits for SQL query to return results
> - `ExternalTaskSensor` — waits for a task in another DAG to complete
> - `TimeSensor` — waits until a specific time
>
> **Two modes:**
> - **Poke mode** — checks condition every N seconds, occupies a worker slot the whole time. Good for short waits.
> - **Reschedule mode** — checks condition, then releases worker slot between checks. Good for long waits. More resource-efficient.
>
> ```python
> from airflow.sensors.filesystem import FileSensor
> from airflow.sensors.python import PythonSensor
>
> # Wait for a file to exist before processing
> wait_for_file = FileSensor(
>     task_id='wait_for_data_file',
>     filepath='/data/daily_sales.csv',
>     poke_interval=60,       # check every 60 seconds
>     timeout=3600,           # timeout after 1 hour
>     mode='reschedule',      # release worker slot between checks
>     dag=dag
> )
>
> wait_for_file >> process_file_task
> ```

---

<a id="q7"></a>
### 🔷 Q7. What is the difference between Operator and Sensor?

| | Operator | Sensor |
|---|---|---|
| Purpose | Performs an action (run code, query DB) | Waits for a condition to be true |
| Execution | Runs once and completes | Keeps polling until condition met or timeout |
| Blocking | Non-blocking — runs and finishes | Blocking — waits and keeps checking |
| Examples | PythonOperator, BashOperator | FileSensor, HttpSensor, S3KeySensor |
| Use case | Execute ETL steps | Wait for upstream data to arrive |
| Timeout | No (completes immediately) | Yes — times out if condition not met |

---

<a id="q8"></a>
### 🔷 Q8. What are the different Executors in Airflow?

> **1. SequentialExecutor:**
> - Runs tasks one at a time — no parallelism
> - Default with SQLite database
> - Only for development and testing
> - Never use in production
>
> **2. LocalExecutor:**
> - Runs tasks in parallel on the same machine as the scheduler
> - Uses Python multiprocessing
> - Good for small to medium workloads on a single machine
> - Requires PostgreSQL or MySQL (not SQLite)
>
> **3. CeleryExecutor:**
> - Distributes tasks across multiple worker machines
> - Uses Celery task queue + Redis or RabbitMQ as message broker
> - Workers can be on different machines — horizontally scalable
> - Best for large-scale production deployments
> - More complex setup
>
> **4. KubernetesExecutor:**
> - Each task runs in its own Kubernetes Pod
> - Complete isolation between tasks
> - Auto-scales — pods created on demand, deleted after task completes
> - Best for cloud-native deployments
> - Resource-efficient — no idle workers
>
> **5. CeleryKubernetesExecutor:**
> - Hybrid — some tasks run on Celery workers, some on Kubernetes pods
> - Flexible routing
>
> **Which executor to use:**
> ```
> Development → SequentialExecutor
> Single machine, medium scale → LocalExecutor
> Multiple machines → CeleryExecutor
> Cloud-native, Kubernetes → KubernetesExecutor
> ```

---

<a id="q9"></a>
### 🔷 Q9. What is the Airflow Scheduler?

> **What the Scheduler does:**
> - Continuously monitors all DAG files in the DAGs folder
> - Checks which DAG runs are due based on schedule_interval
> - Checks task dependencies — triggers tasks whose upstream dependencies are complete
> - Sends ready tasks to the Executor
> - Updates task states in the Metadata Database
>
> **How Scheduler works step by step:**
> - Parses DAG files every 30 seconds (configurable)
> - Checks DAG run schedule — is it time to run?
> - For each DAG run, finds tasks with all dependencies met
> - Marks those tasks as "scheduled"
> - Passes them to the Executor
> - Executor runs the task and reports back status
> - Scheduler updates Metadata DB with new status
>
> **Important Scheduler configs:**
> ```
> scheduler_heartbeat_sec = 5       # how often scheduler checks for work
> min_file_process_interval = 30    # how often DAG files are re-parsed
> dag_file_processor_timeout = 50   # timeout for parsing a DAG file
> ```

---

<a id="q10"></a>
### 🔷 Q10. What are the different Task States in Airflow?

| State | Meaning |
|---|---|
| **none** | Task has not been queued yet |
| **scheduled** | Scheduler has identified it is ready to run |
| **queued** | Sent to Executor, waiting for a worker |
| **running** | Currently being executed by a worker |
| **success** | Task completed successfully |
| **failed** | Task raised an exception or error |
| **up_for_retry** | Failed but retries are configured — will run again |
| **up_for_reschedule** | Sensor task waiting to recheck condition |
| **skipped** | Task was skipped (e.g. by BranchOperator) |
| **upstream_failed** | Upstream task failed — this task won't run |
| **removed** | Task was removed from DAG |

> **Normal flow:**
> ```
> none → scheduled → queued → running → success ✅
>                                      → failed → up_for_retry → running → success ✅
>                                      → skipped (branch condition not met)
> ```

---

## 🟡 SECTION 2: XCOMS, HOOKS & DEPENDENCIES (Q11–Q20)

---

<a id="q11"></a>
### 🔷 Q11. What is XCom in Airflow?

> **Definition:**
> - XCom = Cross-Communication
> - Mechanism to pass small amounts of data between tasks in a DAG
> - Data is stored in the Metadata Database
> - Identified by: key + task_id + dag_id
>
> **How to use XCom:**
> ```python
> from airflow.operators.python import PythonOperator
>
> # Task 1 — PUSH data to XCom
> def extract_data(**kwargs):
>     data = {"rows": 1000, "source": "sales_db"}
>     # Push to XCom
>     kwargs['ti'].xcom_push(key='extract_info', value=data)
>
> # Task 2 — PULL data from XCom
> def transform_data(**kwargs):
>     # Pull from XCom
>     info = kwargs['ti'].xcom_pull(task_ids='extract', key='extract_info')
>     print(f"Processing {info['rows']} rows from {info['source']}")
>
> extract = PythonOperator(task_id='extract', python_callable=extract_data)
> transform = PythonOperator(task_id='transform', python_callable=transform_data)
>
> extract >> transform
> ```
>
> **XCom Limitations — Very Important:**
> - Only for SMALL data — IDs, file paths, counts, status messages
> - NOT for large data — DataFrames, large files, huge JSON
> - Default storage: Metadata DB (PostgreSQL/MySQL) — large data bloats it
> - Max recommended size: a few KB
> - For large data: save to GCS/S3 and pass only the file path via XCom

---

<a id="q12"></a>
### 🔷 Q12. What is the TaskFlow API in Airflow 2.0?

> **Definition:**
> - TaskFlow API introduced in Airflow 2.0 — cleaner, more Pythonic way to write DAGs
> - Uses `@task` decorator instead of creating Operator instances
> - Automatically handles XCom push/pull — no need to manually use ti.xcom_push/pull
> - Dependencies inferred automatically from function calls
>
> **Old way vs TaskFlow API:**
> ```python
> # ❌ Old way — verbose, manual XCom
> def extract(**kwargs):
>     data = get_data()
>     kwargs['ti'].xcom_push(key='data', value=data)
>
> def transform(**kwargs):
>     data = kwargs['ti'].xcom_pull(task_ids='extract', key='data')
>     return process(data)
>
> # ✅ TaskFlow API — clean and simple
> from airflow.decorators import dag, task
> from datetime import datetime
>
> @dag(schedule_interval='@daily', start_date=datetime(2024,1,1), catchup=False)
> def sales_pipeline():
>
>     @task
>     def extract():
>         return {"rows": 1000, "source": "sales_db"}  # auto pushed to XCom
>
>     @task
>     def transform(data: dict):  # auto pulled from XCom
>         return {"cleaned_rows": data["rows"] - 10}
>
>     @task
>     def load(data: dict):
>         print(f"Loading {data['cleaned_rows']} rows")
>
>     # Dependencies inferred automatically
>     raw = extract()
>     clean = transform(raw)
>     load(clean)
>
> dag = sales_pipeline()
> ```
>
> **Benefits of TaskFlow API:**
> - Much cleaner and readable code
> - No boilerplate for XCom push/pull
> - Dependencies auto-inferred from function arguments
> - Type hints supported
> - Easier to test Python functions independently

---

<a id="q13"></a>
### 🔷 Q13. What is the difference between XCom and Variables?

| | XCom | Variables |
|---|---|---|
| Purpose | Pass data between tasks within a DAG run | Store global config values accessible anywhere |
| Scope | One specific DAG run — per task_id, dag_id | Global — accessible by any DAG or task |
| Lifetime | Lives for duration of DAG run | Permanent until deleted |
| Use case | Row count, file path, task output | API keys, table names, environment flags |
| Storage | Metadata Database | Metadata Database |
| Access | `ti.xcom_push/pull` or `@task` auto | `Variable.get('my_key')` |

```python
from airflow.models import Variable

# Set a variable (usually done in UI or CLI)
Variable.set("env", "production")
Variable.set("bq_dataset", "sales_data")

# Get variable in a task
def my_task():
    env = Variable.get("env")
    dataset = Variable.get("bq_dataset")
    print(f"Running in {env}, dataset: {dataset}")
```

---

<a id="q14"></a>
### 🔷 Q14. What are Airflow Hooks?

> **Definition:**
> - Hooks are interfaces that connect Airflow tasks to external systems — databases, cloud services, APIs
> - They handle the low-level connection management code
> - Operators use Hooks internally to connect to external systems
> - Hooks reuse Airflow Connections — no need to hardcode credentials
>
> **Common Hooks:**
> - `PostgresHook` — connect to PostgreSQL
> - `MySqlHook` — connect to MySQL
> - `S3Hook` — read/write files in AWS S3
> - `GCSHook` — read/write files in Google Cloud Storage
> - `BigQueryHook` — interact with BigQuery
> - `HttpHook` — call REST APIs
> - `SlackHook` — send Slack messages
>
> ```python
> from airflow.providers.postgres.hooks.postgres import PostgresHook
> from airflow.providers.google.cloud.hooks.gcs import GCSHook
>
> def export_to_gcs(**kwargs):
>     # Use PostgresHook — credentials from Airflow Connection
>     pg_hook = PostgresHook(postgres_conn_id='my_postgres')
>     df = pg_hook.get_pandas_df("SELECT * FROM orders WHERE date = '2024-01-15'")
>
>     # Save to CSV locally
>     df.to_csv('/tmp/orders.csv', index=False)
>
>     # Use GCSHook — upload to GCS
>     gcs_hook = GCSHook(gcp_conn_id='my_gcp')
>     gcs_hook.upload(bucket_name='my-bucket',
>                     object_name='orders/2024-01-15.csv',
>                     filename='/tmp/orders.csv')
> ```

---

<a id="q15"></a>
### 🔷 Q15. What is the difference between Hooks and Operators?

| | Hooks | Operators |
|---|---|---|
| Purpose | Manage connections to external systems | Define the task logic and actions |
| Level | Low-level interface | High-level abstraction |
| Usage | Used inside Operators or directly in code | Used to define tasks in DAGs |
| Credentials | Reads from Airflow Connections | Uses Hooks internally |
| Example | `PostgresHook.get_records()` | `PostgresOperator(sql='...')` |

> **Simple rule:** Operators use Hooks behind the scenes. If you need custom logic, use Hooks directly. If standard logic works, use Operators.

---

<a id="q16"></a>
### 🔷 Q16. How do you define Task Dependencies in Airflow?

> **Three ways to define dependencies:**
>
> **1. Bitshift operators (most common and clean):**
> ```python
> # >> means "then run"
> extract >> transform >> load
>
> # << means "depends on"
> load << transform << extract
>
> # Multiple dependencies
> [extract_a, extract_b] >> transform >> load
> ```
>
> **2. set_upstream / set_downstream:**
> ```python
> transform.set_upstream(extract)     # transform runs after extract
> transform.set_downstream(load)      # load runs after transform
> ```
>
> **3. chain() function — for complex chains:**
> ```python
> from airflow.models.baseoperator import chain
>
> chain(extract, transform, validate, load, notify)
> # Same as: extract >> transform >> validate >> load >> notify
> ```
>
> **Fan-out and Fan-in:**
> ```python
> # Fan-out: one task triggers multiple
> extract >> [transform_a, transform_b, transform_c]
>
> # Fan-in: multiple tasks merge into one
> [transform_a, transform_b, transform_c] >> load
> ```

---

<a id="q17"></a>
### 🔷 Q17. What is a Trigger Rule in Airflow?

> **Definition:**
> - Trigger Rule defines when a task should run based on the state of its upstream tasks
> - Default: `all_success` — task runs only if all upstream tasks succeeded
>
> **All Trigger Rules:**

| Trigger Rule | When task runs |
|---|---|
| `all_success` (default) | All upstream tasks succeeded |
| `all_failed` | All upstream tasks failed |
| `all_done` | All upstream tasks completed (success or failure) |
| `one_success` | At least one upstream task succeeded |
| `one_failed` | At least one upstream task failed |
| `none_failed` | All upstream tasks succeeded or were skipped |
| `none_skipped` | No upstream tasks were skipped |
| `always` | Run regardless of upstream state |

> ```python
> # Cleanup task always runs — even if pipeline fails
> cleanup = PythonOperator(
>     task_id='cleanup_temp_files',
>     python_callable=cleanup_function,
>     trigger_rule='all_done'  # runs regardless of upstream success/failure
> )
>
> # Alert task runs only if something failed
> alert = PythonOperator(
>     task_id='send_failure_alert',
>     python_callable=send_alert,
>     trigger_rule='one_failed'
> )
> ```

---

<a id="q18"></a>
### 🔷 Q18. What are Airflow Connections?

> **Definition:**
> - Connections store credentials and connection details for external systems
> - Stored securely in the Metadata Database or secret backends (HashiCorp Vault, GCP Secret Manager)
> - Operators and Hooks use connections by `conn_id` — no hardcoded passwords
>
> **Connection fields:**
> - `conn_id` — unique name (e.g. `my_postgres`, `my_gcp`)
> - `conn_type` — database type (postgres, mysql, google_cloud, aws, http)
> - `host` — server hostname or URL
> - `login` — username
> - `password` — password (encrypted in DB)
> - `port` — port number
> - `schema` — database name
>
> **How to create:**
> - Airflow UI: Admin → Connections → Add
> - Airflow CLI: `airflow connections add`
> - Environment variables: `AIRFLOW_CONN_MY_POSTGRES=postgresql://user:pass@host/db`
>
> ```python
> # Use connection in Hook/Operator by conn_id
> pg_operator = PostgresOperator(
>     task_id='run_query',
>     postgres_conn_id='my_postgres',  # references the stored connection
>     sql='SELECT COUNT(*) FROM orders'
> )
> ```

---

<a id="q19"></a>
### 🔷 Q19. What are Airflow Variables?

> **Definition:**
> - Variables are global key-value pairs stored in Airflow's Metadata Database
> - Accessible by any DAG or task at any time
> - Used for configuration values that shouldn't be hardcoded in DAG files
>
> **Common uses:**
> - Environment flag — `env = "production"` or `"staging"`
> - Table names — `bq_table = "project.dataset.orders"`
> - Date ranges — `start_date = "2024-01-01"`
> - API URLs — `api_endpoint = "https://api.example.com"`
>
> ```python
> from airflow.models import Variable
>
> def run_pipeline(**kwargs):
>     # Get variables
>     env = Variable.get("environment", default_var="staging")
>     table = Variable.get("bq_table")
>     batch_size = int(Variable.get("batch_size", default_var="1000"))
>
>     # Use JSON variables
>     config = Variable.get("pipeline_config", deserialize_json=True)
>     print(config['source_db'])
> ```
>
> **Important:** Avoid calling `Variable.get()` at the top level of a DAG file — it runs every time the scheduler parses the file. Call it only inside task functions.

---

<a id="q20"></a>
### 🔷 Q20. What is Jinja Templating in Airflow?

> **Definition:**
> - Jinja is a Python templating engine used in Airflow to make operator fields dynamic
> - Allows using runtime variables like execution date, run ID in SQL queries and bash commands
> - Evaluated at runtime — not when DAG is parsed
>
> **Common Jinja macros:**
> ```python
> {{ ds }}                    # execution date as YYYY-MM-DD
> {{ ds_nodash }}             # execution date as YYYYMMDD
> {{ execution_date }}        # full execution datetime
> {{ next_ds }}               # next execution date
> {{ prev_ds }}               # previous execution date
> {{ run_id }}                # unique run ID
> {{ dag.dag_id }}            # DAG name
> {{ task.task_id }}          # task name
> {{ params.my_param }}       # custom parameter
> ```
>
> ```python
> # Use Jinja in SQL query
> query_task = BigQueryInsertJobOperator(
>     task_id='daily_query',
>     configuration={
>         "query": {
>             "query": """
>                 SELECT region, SUM(amount) as total
>                 FROM orders
>                 WHERE DATE(order_date) = '{{ ds }}'
>                 GROUP BY region
>             """,
>             "useLegacySql": False
>         }
>     }
> )
>
> # Use Jinja in BashOperator
> bash_task = BashOperator(
>     task_id='archive_files',
>     bash_command='mv /data/{{ ds_nodash }}_*.csv /archive/'
> )
> ```

---

## 🟠 SECTION 3: SCHEDULING & ADVANCED (Q21–Q33)

---

<a id="q21"></a>
### 🔷 Q21. What is the catchup parameter in Airflow?

> **Definition:**
> - `catchup` controls whether Airflow should run all past DAG runs that were missed since `start_date`
>
> **catchup=True (default):**
> - Airflow creates a DAG run for every missed interval from start_date to now
> - If you set start_date = 6 months ago with daily schedule → creates 180 DAG runs immediately
> - Useful for backfilling historical data
>
> **catchup=False (recommended for most cases):**
> - Airflow only runs the most recent interval
> - Does not create missed runs from the past
> - Use this for most production pipelines to avoid overloading the system
>
> ```python
> with DAG(
>     dag_id='daily_pipeline',
>     start_date=datetime(2024, 1, 1),
>     schedule_interval='@daily',
>     catchup=False  # ✅ recommended — don't run all past dates
> ) as dag:
>     ...
> ```

---

<a id="q22"></a>
### 🔷 Q22. What is Backfilling in Airflow?

> **Definition:**
> - Backfilling is running DAG runs for past dates that were not executed before
> - Useful when you add a new pipeline and need to process historical data
>
> **Methods to backfill:**
> ```bash
> # Using Airflow CLI
> airflow dags backfill \
>     --start-date 2024-01-01 \
>     --end-date 2024-01-31 \
>     daily_sales_pipeline
>
> # Backfill with rerun of failed tasks only
> airflow dags backfill \
>     --start-date 2024-01-01 \
>     --end-date 2024-01-31 \
>     --rerun-failed-tasks \
>     daily_sales_pipeline
> ```
>
> **Important considerations:**
> - Make sure DAG is idempotent — running multiple times gives same result
> - Backfilling can create many concurrent runs — may overload resources
> - Use `max_active_runs` to limit concurrent DAG runs during backfill
> - `catchup=True` + `start_date` in the past = automatic backfill

---

<a id="q23"></a>
### 🔷 Q23. What is the execution_date in Airflow?

> **Definition:**
> - `execution_date` is the logical date for which a DAG run is processing data
> - NOT the actual time the DAG ran — it is the start of the schedule interval
>
> **Example:**
> - DAG schedule: `@daily` (runs every day)
> - DAG run triggered on January 16, 2024 at 2 AM
> - `execution_date` = January 15, 2024 (the interval it is processing)
> - This is intentional — daily job runs at midnight to process previous day's data
>
> **In Airflow 2.2+:**
> - `execution_date` renamed to `logical_date` (more accurate name)
> - `data_interval_start` and `data_interval_end` also available
>
> ```python
> def process_data(**kwargs):
>     exec_date = kwargs['execution_date']
>     logical_date = kwargs['logical_date']
>     data_start = kwargs['data_interval_start']
>     data_end = kwargs['data_interval_end']
>
>     print(f"Processing data for: {exec_date.date()}")
>
> # Access in template
> # {{ ds }} = execution_date as YYYY-MM-DD string
> ```

---

<a id="q24"></a>
### 🔷 Q24. What is a SubDAG and TaskGroup?

> **SubDAG (old approach — deprecated):**
> - A DAG nested inside another DAG
> - Groups tasks visually in the UI
> - Had performance issues and complexity — no longer recommended
>
> **TaskGroup (new approach — recommended):**
> - Groups tasks visually within a DAG — shown as a collapsed box in UI
> - No performance overhead
> - Tasks still run in the parent DAG context
> - Available from Airflow 2.0+
>
> ```python
> from airflow.utils.task_group import TaskGroup
>
> with DAG('pipeline', schedule_interval='@daily', ...) as dag:
>
>     # Group extraction tasks
>     with TaskGroup('extract_group') as extract_group:
>         extract_sales = PythonOperator(task_id='extract_sales', ...)
>         extract_inventory = PythonOperator(task_id='extract_inventory', ...)
>
>     # Group transformation tasks
>     with TaskGroup('transform_group') as transform_group:
>         clean_sales = PythonOperator(task_id='clean_sales', ...)
>         clean_inventory = PythonOperator(task_id='clean_inventory', ...)
>
>     load = PythonOperator(task_id='load', ...)
>
>     # Set group-level dependencies
>     extract_group >> transform_group >> load
> ```

---

<a id="q25"></a>
### 🔷 Q25. What is BranchPythonOperator in Airflow?

> **Definition:**
> - BranchPythonOperator allows conditional execution — run different tasks based on a condition
> - The function must return the `task_id` of the next task to run
> - Unselected branches are marked as SKIPPED
>
> ```python
> from airflow.operators.python import BranchPythonOperator
>
> def decide_branch(**kwargs):
>     exec_date = kwargs['execution_date']
>     # Run heavy processing only on weekends
>     if exec_date.weekday() >= 5:  # 5=Saturday, 6=Sunday
>         return 'heavy_processing'
>     else:
>         return 'light_processing'
>
> branch = BranchPythonOperator(
>     task_id='decide_processing',
>     python_callable=decide_branch
> )
>
> heavy = PythonOperator(task_id='heavy_processing', ...)
> light = PythonOperator(task_id='light_processing', ...)
> merge = PythonOperator(
>     task_id='merge_results',
>     trigger_rule='none_failed'  # runs after either branch completes
> )
>
> branch >> [heavy, light] >> merge
> ```

---

<a id="q26"></a>
### 🔷 Q26. What are Airflow Pools?

> **Definition:**
> - Pools control the parallelism of tasks by limiting how many tasks can run simultaneously
> - Each pool has a fixed number of slots — tasks consume slots when running
> - Used to prevent overloading external systems with too many concurrent connections
>
> **Use cases:**
> - Limit concurrent database connections — prevent overwhelming PostgreSQL
> - Limit API calls — respect rate limits of external APIs
> - Limit BigQuery concurrent jobs
>
> ```python
> # Assign task to a pool
> extract_task = PythonOperator(
>     task_id='extract_from_db',
>     python_callable=extract,
>     pool='database_pool',   # task uses a slot from database_pool
>     pool_slots=1            # consumes 1 slot
> )
> ```
>
> **Create a pool:**
> - UI: Admin → Pools → Add pool (name + number of slots)
> - CLI: `airflow pools set database_pool 5 "Max 5 DB connections"`
> - If pool is full, new tasks wait in queue until a slot frees up

---

<a id="q27"></a>
### 🔷 Q27. What are Airflow Queues?

> **Definition:**
> - Queues are used with CeleryExecutor to route tasks to specific worker groups
> - Different workers can be assigned to different queues
> - Useful for routing tasks to workers with specific hardware or software
>
> **Use cases:**
> - Route ML tasks to GPU workers
> - Route database tasks to workers with DB access
> - Route BigQuery tasks to workers with GCP credentials
>
> ```python
> # Route task to specific worker queue
> ml_task = PythonOperator(
>     task_id='train_model',
>     python_callable=train,
>     queue='gpu_workers'  # only runs on workers listening to gpu_workers queue
> )
>
> db_task = PythonOperator(
>     task_id='export_data',
>     python_callable=export,
>     queue='db_workers'   # only runs on workers with DB access
> )
> ```

---

<a id="q28"></a>
### 🔷 Q28. How does Airflow handle Retries?

> **Retry configuration:**
> ```python
> default_args = {
>     'retries': 3,                              # retry 3 times
>     'retry_delay': timedelta(minutes=5),       # wait 5 min between retries
>     'retry_exponential_backoff': True,         # wait longer each retry (5, 10, 20 min)
>     'max_retry_delay': timedelta(hours=1),     # max wait between retries
> }
>
> # Override per task
> risky_task = PythonOperator(
>     task_id='call_api',
>     python_callable=call_external_api,
>     retries=5,
>     retry_delay=timedelta(minutes=2)
> )
> ```
>
> **Retry flow:**
> ```
> Task fails → state: "up_for_retry" → wait retry_delay
>           → retry 1 → fails again → wait → retry 2 → ...
>           → after all retries exhausted → state: "failed"
> ```
>
> **Best practices:**
> - Set retries for tasks that call external APIs or databases — transient failures are common
> - Use `retry_exponential_backoff=True` to avoid hammering failing services
> - Set `email_on_retry=False` to avoid too many retry emails

---

<a id="q29"></a>
### 🔷 Q29. What is DAG Serialization in Airflow?

> **Definition:**
> - DAG Serialization stores serialized (JSON) versions of DAGs in the Metadata Database
> - Introduced to solve performance issues caused by the webserver parsing all DAG files on every page load
>
> **Without serialization (old):**
> - Webserver parsed all DAG Python files every time UI was loaded
> - With 100s of DAGs this was very slow
> - Required DAG files to be on the webserver machine
>
> **With serialization (current default):**
> - Scheduler parses DAG files and saves JSON to Metadata DB
> - Webserver reads JSON from DB — no need to parse Python files
> - Webserver doesn't need access to DAG files
> - Much faster UI loading
> - Enabled by default in Airflow 2.0+

---

<a id="q30"></a>
### 🔷 Q30. What is the Airflow Metadata Database?

> **Definition:**
> - Central database that stores all Airflow state and configuration
> - Everything Airflow knows is stored here
>
> **What it stores:**
> - DAG runs and their states
> - Task instances and their states
> - XCom values passed between tasks
> - Connections (database credentials)
> - Variables (global config values)
> - Pools and their slot counts
> - User information (for authentication)
> - Serialized DAGs (JSON)
>
> **Supported databases:**
> - SQLite — development only, no parallelism
> - PostgreSQL — recommended for production (most stable)
> - MySQL — supported for production
>
> **Important:**
> - Never use SQLite in production
> - Database performance affects all of Airflow — use a well-tuned PostgreSQL
> - Regular maintenance needed — old DAG runs and XComs can grow large
> - Use `airflow db clean` to remove old records

---

<a id="q31"></a>
### 🔷 Q31. What is Dynamic DAG Generation?

> **Definition:**
> - Creating multiple DAGs or tasks programmatically based on configuration or data
> - Instead of writing 10 separate DAG files, generate them in a loop
>
> **Use case:** You have 10 different data sources — create one DAG per source
>
> ```python
> from airflow import DAG
> from airflow.operators.python import PythonOperator
> from datetime import datetime
>
> # Config for multiple sources
> sources = [
>     {"name": "sales_db", "table": "orders"},
>     {"name": "inventory_db", "table": "products"},
>     {"name": "marketing_db", "table": "campaigns"},
> ]
>
> # Generate one DAG per source dynamically
> for source in sources:
>     dag_id = f"etl_{source['name']}"
>
>     with DAG(
>         dag_id=dag_id,
>         start_date=datetime(2024, 1, 1),
>         schedule_interval='@daily',
>         catchup=False
>     ) as dag:
>
>         def extract(table=source['table'], **kwargs):
>             print(f"Extracting from {table}")
>
>         extract_task = PythonOperator(
>             task_id=f"extract_{source['name']}",
>             python_callable=extract
>         )
>
>     # Register in global namespace
>     globals()[dag_id] = dag
> ```

---

<a id="q32"></a>
### 🔷 Q32. What is Airflow's CeleryExecutor and how does it work?

> **Architecture:**
> ```
> Scheduler → Message Broker (Redis/RabbitMQ) → Celery Workers → Execute Tasks
>                                                              → Report status back to Metadata DB
> ```
>
> **How it works step by step:**
> - Scheduler identifies ready tasks and sends them to the message broker
> - Message broker (Redis or RabbitMQ) queues the tasks
> - Multiple Celery workers listen to the queue
> - Available worker picks up a task and executes it
> - Worker updates task status in Metadata DB on completion
> - Celery Flower (optional) provides UI to monitor workers
>
> **Benefits:**
> - Horizontally scalable — add more workers to handle more tasks
> - Workers can run on different machines
> - Failed worker doesn't lose tasks — broker retains them
>
> **Setup requires:**
> - Redis or RabbitMQ as message broker
> - Multiple worker machines or containers
> - Shared file system or DAG deployment strategy for workers

---

<a id="q33"></a>
### 🔷 Q33. What is KubernetesExecutor in Airflow?

> **How it works:**
> - Each task creates a new Kubernetes Pod — runs in complete isolation
> - Pod is deleted after task completes
> - No idle workers — pods created on demand only
>
> **Benefits:**
> - Complete task isolation — one task's failure doesn't affect others
> - Resource efficiency — no idle workers sitting around
> - Each task can have its own Docker image and resource requirements
> - Auto-scales perfectly — Kubernetes handles pod scheduling
>
> **Compared to CeleryExecutor:**

| | CeleryExecutor | KubernetesExecutor |
|---|---|---|
| Setup | Medium complexity | Higher complexity |
| Isolation | Shared worker process | Full pod isolation |
| Scaling | Add/remove workers | Auto (pod per task) |
| Cold start | Fast (workers already running) | Slower (pod startup time) |
| Resource usage | Idle workers consume resources | No idle resources |
| Best for | Traditional servers, VMs | Cloud-native, Kubernetes clusters |

---

## 🔴 SECTION 4: PRODUCTION & BEST PRACTICES (Q34–Q45)

---

<a id="q34"></a>
### 🔷 Q34. What is the difference between CeleryExecutor and KubernetesExecutor?

> *(See table in Q33 above)*
>
> **When to use which:**
> - Use **CeleryExecutor** when:
>   - Already using Redis/RabbitMQ
>   - Running on VMs or traditional servers
>   - Task startup speed is important
>   - Simple, predictable workloads
>
> - Use **KubernetesExecutor** when:
>   - Running on Kubernetes cluster (GKE, EKS, AKS)
>   - Need strict task isolation
>   - Tasks have very different resource requirements
>   - Want zero idle resource cost
>   - Cloud-native data platform

---

<a id="q35"></a>
### 🔷 Q35. How do you make a DAG Idempotent?

> **What is idempotency:**
> - Running the same DAG run multiple times gives the same result — no duplicates, no data corruption
> - Critical for reliable pipelines — retries and backfills must not cause issues
>
> **How to achieve idempotency:**
>
> **1. Use INSERT ... ON CONFLICT (UPSERT) instead of plain INSERT:**
> ```sql
> INSERT INTO orders (id, amount, region)
> VALUES (101, 5000, 'East')
> ON CONFLICT (id) DO UPDATE SET amount = EXCLUDED.amount;
> ```
>
> **2. Delete before insert — partition replacement:**
> ```python
> def load_data(exec_date, **kwargs):
>     # Delete existing data for this date first
>     pg_hook.run(f"DELETE FROM orders WHERE date = '{exec_date}'")
>     # Then insert fresh data
>     pg_hook.insert_rows('orders', rows)
> ```
>
> **3. Use BigQuery WRITE_TRUNCATE for daily partitions:**
> ```python
> job_config = bigquery.LoadJobConfig(
>     write_disposition=bigquery.WriteDisposition.WRITE_TRUNCATE
> )
> ```
>
> **4. Check if already processed — skip if done:**
> ```python
> def extract(**kwargs):
>     exec_date = kwargs['ds']
>     # Check if already processed
>     count = pg_hook.get_first(
>         f"SELECT COUNT(*) FROM audit WHERE date = '{exec_date}'"
>     )[0]
>     if count > 0:
>         print("Already processed — skipping")
>         return
>     # Process data...
> ```

---

<a id="q36"></a>
### 🔷 Q36. How do you handle failures and alerts in Airflow?

> **1. Email alerts:**
> ```python
> default_args = {
>     'email': ['rajkishor@example.com', 'team@example.com'],
>     'email_on_failure': True,    # email when task fails
>     'email_on_retry': False,     # don't email on every retry
>     'email_on_success': False,   # don't email on success
> }
> ```
>
> **2. Custom callback functions:**
> ```python
> def on_failure_callback(context):
>     dag_id = context['dag'].dag_id
>     task_id = context['task_instance'].task_id
>     exec_date = context['execution_date']
>     error = context['exception']
>
>     # Send Slack notification
>     message = f"❌ FAILED: {dag_id}.{task_id} on {exec_date}\nError: {error}"
>     send_slack_message(message)
>
> def on_success_callback(context):
>     print("Pipeline completed successfully!")
>
> with DAG(
>     dag_id='my_pipeline',
>     on_failure_callback=on_failure_callback,   # DAG-level callback
>     ...
> ) as dag:
>
>     critical_task = PythonOperator(
>         task_id='critical',
>         python_callable=my_func,
>         on_failure_callback=on_failure_callback,   # task-level callback
>         on_success_callback=on_success_callback
>     )
> ```
>
> **3. SLA (Service Level Agreement):**
> ```python
> with DAG(
>     dag_id='sla_pipeline',
>     sla_miss_callback=sla_miss_handler,  # called if SLA missed
>     ...
> ) as dag:
>
>     task = PythonOperator(
>         task_id='process',
>         python_callable=process,
>         sla=timedelta(hours=2)  # task must complete within 2 hours
>     )
> ```

---

<a id="q37"></a>
### 🔷 Q37. What is an Airflow DAG Run?

> **Definition:**
> - A DAG Run is one execution instance of a DAG
> - Every time a DAG runs (scheduled or manually triggered), it creates a new DAG Run
> - Each DAG Run has a unique `run_id` and `execution_date`
>
> **DAG Run types:**
> - `scheduled` — triggered by the scheduler based on schedule_interval
> - `manual` — triggered manually via UI, CLI or API
> - `backfill` — triggered via backfill command for past dates
>
> **DAG Run states:**
> - `running` — currently executing
> - `success` — all tasks completed successfully
> - `failed` — at least one task failed and not retried
>
> **Control concurrent DAG runs:**
> ```python
> with DAG(
>     dag_id='my_pipeline',
>     max_active_runs=1,    # only 1 DAG run at a time — prevents overlapping runs
>     ...
> ) as dag:
>     ...
> ```

---

<a id="q38"></a>
### 🔷 Q38. What is the difference between schedule_interval and timetable?

> **schedule_interval (traditional):**
> - Uses cron expressions or preset strings
> - Simple and commonly used
>
> ```python
> schedule_interval='@daily'          # every day at midnight
> schedule_interval='@hourly'         # every hour
> schedule_interval='@weekly'         # every Monday at midnight
> schedule_interval='0 2 * * *'       # every day at 2 AM
> schedule_interval='0 8 * * 1-5'     # 8 AM Monday to Friday
> schedule_interval=timedelta(hours=6) # every 6 hours
> ```
>
> **Cron expression format:**
> ```
> * * * * *
> │ │ │ │ │
> │ │ │ │ └── Day of week (0-7, 0=Sunday)
> │ │ │ └──── Month (1-12)
> │ │ └────── Day of month (1-31)
> │ └──────── Hour (0-23)
> └────────── Minute (0-59)
> ```
>
> **Timetable (Airflow 2.2+):**
> - More flexible scheduling — custom logic not possible with cron
> - Example: run only on business days, skip holidays
> - Written as a Python class

---

<a id="q39"></a>
### 🔷 Q39. How do you pass parameters to a DAG at runtime?

> **Method 1 — Using conf (dagrun.conf):**
> ```python
> # Trigger with parameters from CLI
> airflow dags trigger my_dag --conf '{"date": "2024-01-15", "region": "East"}'
>
> # Access conf in task
> def process(**kwargs):
>     conf = kwargs['dag_run'].conf
>     date = conf.get('date', '2024-01-01')
>     region = conf.get('region', 'All')
>     print(f"Processing {region} for {date}")
> ```
>
> **Method 2 — Using Params (Airflow 2.2+):**
> ```python
> from airflow.models.param import Param
>
> with DAG(
>     dag_id='parameterized_pipeline',
>     params={
>         'region': Param('East', type='string'),
>         'batch_size': Param(1000, type='integer')
>     },
>     ...
> ) as dag:
>
>     def process(**kwargs):
>         region = kwargs['params']['region']
>         batch_size = kwargs['params']['batch_size']
> ```
>
> **Method 3 — Using Variables:**
> ```python
> from airflow.models import Variable
> region = Variable.get("target_region", default_var="All")
> ```

---

<a id="q40"></a>
### 🔷 Q40. What are Airflow Best Practices for Data Engineers?

> **DAG Design:**
> - Keep tasks small and focused — one task, one responsibility
> - Make all tasks idempotent — safe to retry multiple times
> - Set `catchup=False` unless backfilling is needed
> - Set `max_active_runs=1` to prevent overlapping pipeline runs
> - Use TaskGroup instead of SubDAGs
> - Use meaningful task_ids — `extract_sales_data` not `task_1`
>
> **Performance:**
> - Avoid heavy computation in DAG file top-level — only define structure
> - Never call `Variable.get()` outside of task functions
> - Use Sensors in `reschedule` mode for long waits — saves worker slots
> - Set appropriate `pool` limits to protect external systems
>
> **Error Handling:**
> - Always configure retries for tasks that call external services
> - Use `on_failure_callback` to send alerts (Slack, email, PagerDuty)
> - Set SLAs for critical pipelines
> - Use `trigger_rule='all_done'` for cleanup tasks
>
> **Data Handling:**
> - Use XCom only for metadata — never pass DataFrames through XCom
> - For large data between tasks — use intermediate storage (GCS, S3, temp table)
> - Always partition destination tables to support idempotent loads
>
> **Security:**
> - Store credentials in Airflow Connections — never hardcode in DAG files
> - Use secret backends (GCP Secret Manager, HashiCorp Vault) for sensitive values
> - Set appropriate permissions on DAG files
>
> **Maintenance:**
> - Regularly clean old DAG runs — `airflow db clean`
> - Monitor Metadata DB size — XComs can grow large
> - Set `max_active_tasks` to control parallelism

---

<a id="q41"></a>
### 🔷 Q41. What is the difference between Airflow and other orchestrators?

| | Apache Airflow | Prefect | Dagster | Luigi |
|---|---|---|---|---|
| Created by | Airbnb (2014) | Prefect Technologies | Elementl | Spotify (2011) |
| DAG definition | Python code in files | Python decorators | Python with assets | Python classes |
| UI | Rich UI (built-in) | Modern cloud UI | Rich UI with asset catalog | Basic UI |
| Learning curve | Medium-High | Low-Medium | Medium-High | Medium |
| Scalability | High (Celery/K8s) | High | High | Medium |
| Cloud support | Any | Native cloud | Any | Any |
| Best for | Complex ETL pipelines | Modern data teams, cloud-first | Data asset management | Simple workflows |
| Popularity | Most widely used | Growing fast | Growing fast | Less popular now |

> **Why Airflow is still most popular:**
> - Largest community and ecosystem
> - Most operators and providers available
> - Most tutorials and documentation
> - Most job postings require Airflow knowledge
> - Battle-tested in production at thousands of companies

---

<a id="q42"></a>
### 🔷 Q42. How do you deploy Airflow in production?

> **Option 1 — Docker Compose (small teams):**
> ```yaml
> # docker-compose.yml
> services:
>   postgres:
>     image: postgres:13
>   redis:
>     image: redis:latest
>   airflow-webserver:
>     image: apache/airflow:2.7.0
>   airflow-scheduler:
>     image: apache/airflow:2.7.0
>   airflow-worker:
>     image: apache/airflow:2.7.0
> ```
>
> **Option 2 — Kubernetes with Helm (production):**
> ```bash
> helm repo add apache-airflow https://airflow.apache.org
> helm install airflow apache-airflow/airflow \
>     --set executor=KubernetesExecutor \
>     --set postgresql.enabled=true
> ```
>
> **Option 3 — Managed Airflow (easiest):**
> - **Google Cloud Composer** — managed Airflow on GCP
> - **Amazon MWAA** — managed Airflow on AWS
> - **Astronomer** — managed Airflow platform
>
> **Production checklist:**
> - Use PostgreSQL as Metadata DB — not SQLite
> - Use CeleryExecutor or KubernetesExecutor — not Sequential
> - Set up Redis/RabbitMQ for Celery
> - Configure email or Slack alerts
> - Set up DAG versioning with Git
> - Configure log storage (GCS, S3)

---

<a id="q43"></a>
### 🔷 Q43. How does Airflow integrate with GCP BigQuery?

> ```python
> from airflow import DAG
> from airflow.providers.google.cloud.operators.bigquery import (
>     BigQueryInsertJobOperator,
>     BigQueryCheckOperator,
>     BigQueryCreateEmptyTableOperator
> )
> from airflow.providers.google.cloud.transfers.gcs_to_bigquery import GCSToBigQueryOperator
> from datetime import datetime
>
> with DAG('bigquery_pipeline', start_date=datetime(2024,1,1),
>          schedule_interval='@daily', catchup=False) as dag:
>
>     # Load CSV from GCS to BigQuery
>     load_data = GCSToBigQueryOperator(
>         task_id='load_gcs_to_bq',
>         bucket='my-bucket',
>         source_objects=['data/{{ ds_nodash }}_orders.csv'],
>         destination_project_dataset_table='project.dataset.orders',
>         write_disposition='WRITE_TRUNCATE',
>         source_format='CSV',
>         skip_leading_rows=1,
>         autodetect=True,
>         gcp_conn_id='google_cloud_default'
>     )
>
>     # Run transformation query
>     transform = BigQueryInsertJobOperator(
>         task_id='transform_data',
>         configuration={
>             "query": {
>                 "query": """
>                     CREATE OR REPLACE TABLE project.dataset.daily_summary AS
>                     SELECT DATE(order_date) as date, region, SUM(amount) as total
>                     FROM project.dataset.orders
>                     WHERE DATE(order_date) = '{{ ds }}'
>                     GROUP BY 1, 2
>                 """,
>                 "useLegacySql": False
>             }
>         }
>     )
>
>     # Validate result
>     check = BigQueryCheckOperator(
>         task_id='check_data_loaded',
>         sql="SELECT COUNT(*) FROM project.dataset.daily_summary WHERE date = '{{ ds }}'",
>         use_legacy_sql=False
>     )
>
>     load_data >> transform >> check
> ```

---

<a id="q44"></a>
### 🔷 Q44. How does Airflow integrate with Apache Kafka?

> ```python
> from airflow.operators.python import PythonOperator
> from kafka import KafkaProducer, KafkaConsumer
> import json
>
> def publish_to_kafka(**kwargs):
>     """Publish pipeline status to Kafka topic"""
>     producer = KafkaProducer(
>         bootstrap_servers=['kafka:9092'],
>         value_serializer=lambda v: json.dumps(v).encode('utf-8')
>     )
>     message = {
>         "pipeline": "daily_sales",
>         "status": "completed",
>         "date": kwargs['ds'],
>         "records": 5000
>     }
>     producer.send('pipeline-events', message)
>     producer.flush()
>     print("Event published to Kafka")
>
> def consume_from_kafka(**kwargs):
>     """Read messages from Kafka before processing"""
>     consumer = KafkaConsumer(
>         'source-events',
>         bootstrap_servers=['kafka:9092'],
>         auto_offset_reset='earliest',
>         group_id='airflow-pipeline'
>     )
>     messages = []
>     for msg in consumer:
>         messages.append(json.loads(msg.value))
>         if len(messages) >= 100:
>             break
>     return messages
>
> publish_task = PythonOperator(
>     task_id='publish_completion',
>     python_callable=publish_to_kafka
> )
> ```

---

<a id="q45"></a>
### 🔷 Q45. What is a real-world ETL pipeline using Airflow?

> **Complete daily sales ETL pipeline:**
>
> ```python
> from airflow.decorators import dag, task
> from airflow.providers.postgres.hooks.postgres import PostgresHook
> from airflow.providers.google.cloud.hooks.gcs import GCSHook
> from airflow.providers.google.cloud.transfers.gcs_to_bigquery import GCSToBigQueryOperator
> from datetime import datetime, timedelta
> import pandas as pd
>
> @dag(
>     dag_id='daily_sales_etl',
>     start_date=datetime(2024, 1, 1),
>     schedule_interval='0 2 * * *',   # 2 AM daily
>     catchup=False,
>     default_args={
>         'retries': 3,
>         'retry_delay': timedelta(minutes=5),
>         'email_on_failure': True,
>         'email': ['data-team@company.com']
>     }
> )
> def sales_pipeline():
>
>     @task
>     def extract_from_postgres(ds):
>         """Extract yesterday's sales from PostgreSQL"""
>         pg_hook = PostgresHook(postgres_conn_id='prod_postgres')
>         df = pg_hook.get_pandas_df(
>             f"SELECT * FROM sales WHERE date = '{ds}'"
>         )
>         print(f"Extracted {len(df)} rows for {ds}")
>         return len(df)  # return count via XCom
>
>     @task
>     def validate_data(row_count: int, ds: str):
>         """Validate extracted data quality"""
>         if row_count == 0:
>             raise ValueError(f"No data extracted for {ds}!")
>         print(f"Validation passed: {row_count} rows")
>         return True
>
>     @task
>     def upload_to_gcs(ds: str):
>         """Upload CSV to GCS"""
>         gcs_hook = GCSHook(gcp_conn_id='google_cloud_default')
>         gcs_hook.upload(
>             bucket_name='my-data-bucket',
>             object_name=f'sales/{ds}/orders.csv',
>             filename=f'/tmp/orders_{ds}.csv'
>         )
>         return f'gs://my-data-bucket/sales/{ds}/orders.csv'
>
>     @task
>     def notify_success(ds: str):
>         print(f"Pipeline completed for {ds} ✅")
>
>     # Define pipeline flow
>     row_count = extract_from_postgres()
>     is_valid = validate_data(row_count)
>     gcs_path = upload_to_gcs()
>     notify_success()
>
> dag = sales_pipeline()
> ```
>
> **This pipeline:**
> - Extracts data from PostgreSQL
> - Validates data quality
> - Uploads to GCS
> - Loads to BigQuery
> - Sends notification on success or failure
> - Retries 3 times on failure
> - Runs daily at 2 AM automatically

---

## 💡 FINAL QUICK REVISION

**Core Concepts:**
> ✅ Airflow = orchestration tool — schedules and monitors data pipelines
> ✅ DAG = Directed Acyclic Graph — defines pipeline as code in Python
> ✅ 4 components: Scheduler, Executor, Web Server, Metadata Database
> ✅ Task = instance of an Operator running in a DAG
> ✅ Default Metadata DB for production = PostgreSQL

**Operators & Sensors:**
> ✅ Operator = performs an action | Sensor = waits for a condition
> ✅ Most used: PythonOperator, BashOperator, BigQueryInsertJobOperator
> ✅ Sensor modes: poke (holds worker) vs reschedule (releases worker)
> ✅ BranchPythonOperator = conditional execution, returns task_id to run

**Executors:**
> ✅ SequentialExecutor = dev only | LocalExecutor = single machine
> ✅ CeleryExecutor = multiple workers via Redis/RabbitMQ
> ✅ KubernetesExecutor = one pod per task, cloud-native

**Data Passing:**
> ✅ XCom = pass small data between tasks (only metadata, not DataFrames)
> ✅ Variables = global config values accessible by any DAG
> ✅ TaskFlow API (@task decorator) = cleaner way to write DAGs in Airflow 2.0
> ✅ For large data = save to GCS/S3 and pass file path via XCom

**Scheduling:**
> ✅ catchup=False = don't run past missed runs (recommended)
> ✅ execution_date = logical date of the data being processed (not run time)
> ✅ Backfill = run DAG for past dates using CLI
> ✅ Jinja templates: {{ ds }} = execution date, {{ run_id }} = unique run ID

**Dependencies & Triggers:**
> ✅ `>>` = set downstream | `<<` = set upstream
> ✅ Default trigger_rule = all_success
> ✅ Use trigger_rule='all_done' for cleanup tasks
> ✅ Pools = limit concurrent tasks to protect external systems

**Best Practices:**
> ✅ Make DAGs idempotent — safe to retry multiple times
> ✅ Set retries=3 for tasks calling external APIs or databases
> ✅ Use on_failure_callback for Slack/email alerts
> ✅ Never hardcode credentials — use Connections
> ✅ Never call Variable.get() at DAG top level — call inside task functions

---

*All the best Rajkishor! Python + SQL + Tableau + BigQuery + Airflow = JSW Data Engineer fully ready! 🎯*
