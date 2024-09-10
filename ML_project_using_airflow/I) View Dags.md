# View your DAGs and Jobs

Congratulations! Finally, you can run the Airflow program, and you can check the DAGs in the graph tab. You can also trigger a job to run manually using the play button. twemoji-25b6

You can also try below code to schedule a job at a particular time.

Change the date in the below code.

```
    'owner': 'airflow',
    'start_date': datetime(2023, 4, 25),
}
```
![](https://cf-courses-data.s3.us.cloud-object-storage.appdomain.cloud/IND-GPXX0DNQEN/images/dags_run_success.png)

After successful running your DAG the status of DAG to successfull as you can see in above image.
You can also check particular dag’s information.

1. Status: The status of a task indicates its current state. When you hover over a task node, you may see the task’s current status, such as “success,” “failed,” “running,” or “skipped.” This information helps monitor the progress and outcomes of task executions.

2. Task ID: The task ID is a unique identifier for each task instance. When you hover over a task node, you may see the task ID associated with that task instance. This ID is useful for tracking and troubleshooting specific task executions.

3. Run: It represents the specific date and time when a task instance was executed. When you hover over a task node, you may see the execution date associated with that task instance. This information is helpful for identifying when a task was executed and tracking the history of task runs.

4. Run ID: The default run ID is generated automatically for each DAG run. The run ID is a unique identifier that helps track and manage individual DAG runs. The default run ID follows the format {DAG_ID}_{EXECUTION_DATE}, where:

{DAG_ID} is the ID of the DAG.
{EXECUTION_DATE} is the date and time when the DAG run started.

5. Python Operator: The PythonOperator is used to execute arbitrary Python functions as tasks within a DAG. It allows you to define a Python function that will be executed when the task runs


![](https://cf-courses-data.s3.us.cloud-object-storage.appdomain.cloud/IND-GPXX0DNQEN/images/1dag_info.png)


* You can also check the particular dags’s variables values which we are passing uing xcom function.
In below image you can see we are passing values of X_train, y_train , X_test, y_test from train_iris_model_2 to train_iris_model_3
* The push_value function is defined as the python_callable for the PythonOperator. Inside the function, we set a value of X_train, y_train , X_test, y_test and use xcom_push to store it in XCom with the key.


![](https://cf-courses-data.s3.us.cloud-object-storage.appdomain.cloud/IND-GPXX0DNQEN/images/xcom_info.png)


-> You can also check the list of jobs from Browse button jobs tab.
![](https://cf-courses-data.s3.us.cloud-object-storage.appdomain.cloud/IND-GPXX0DNQEN/images/job_list_dags.png)
