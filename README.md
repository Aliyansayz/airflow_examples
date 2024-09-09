# airflow_examples

AirFlow Concepts
https://chatgpt.com/share/8b361142-ecff-4c26-9390-b2df7c284bb2


Explanation
Extract Task:
Simulates extracting data from a CSV file and saves it to /tmp/data.csv.

```python
from airflow import DAG
from airflow.operators.python_operator import PythonOperator
from datetime import datetime, timedelta
import pandas as pd
import sqlite3

# Default arguments for the DAG
default_args = {
    'owner': 'airflow',
    'depends_on_past': False,
    'start_date': datetime(2023, 1, 1),
    'email_on_failure': False,
    'email_on_retry': False,
    'retries': 1,
    'retry_delay': timedelta(minutes=5),
}

# Define the DAG
dag = DAG(
    'example_etl_dag',
    default_args=default_args,
    description='An example ETL DAG',
    schedule_interval=timedelta(days=1),
)

# Define the tasks
def extract():
    # Simulate extracting data from a CSV file
    data = {'name': ['John', 'Jane', 'Doe'], 'age': [28, 34, 29]}
    df = pd.DataFrame(data)
    df.to_csv('/tmp/data.csv', index=False)


# Create the tasks
extract_task = PythonOperator(
    task_id='extract',
    python_callable=extract,
    dag=dag,
)

```


Transform Task:
Reads the extracted data, performs a simple transformation (increments the age by 1), and saves the transformed data to /tmp/transformed_data.csv.

```python
def transform():
    # Simulate transforming data
    df = pd.read_csv('/tmp/data.csv')
    df['age'] = df['age'] + 1
    df.to_csv('/tmp/transformed_data.csv', index=False)


transform_task = PythonOperator(
    task_id='transform',
    python_callable=transform,
    dag=dag,
)

```


Load Task:
Loads the transformed data into a SQLite database.

```python

def load():
    # Simulate loading data into a database
    df = pd.read_csv('/tmp/transformed_data.csv')
    conn = sqlite3.connect('/tmp/example.db')
    df.to_sql('people', conn, if_exists='replace', index=False)
    conn.close()

load_task = PythonOperator(
    task_id='load',
    python_callable=load,
    dag=dag,
)
```

Task Dependencies:
The tasks are set to run in sequence: extract -> transform -> load.

```python
# Set task dependencies
extract_task >> transform_task >> load_task

```

This is a basic example to get you started. You can expand it by adding more complex transformations, using different data sources, and integrating with other systems
