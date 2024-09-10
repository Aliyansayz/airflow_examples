# Define the Dags


```python

  default_args = {
    'owner': 'airflow',
    'start_date': datetime(2023, 5, 22),
}
# Here we have created 1 main tag iris_classification_models and 3 tasks
with DAG('iris_classification_models', default_args=default_args, schedule_interval=None) as dag:
    iris_classification_model_1 = PythonOperator(
        task_id='train_iris_classification_model_1',
        python_callable=train_iris_model_1,
    )
    iris_classification_model_2 = PythonOperator(
        task_id='train_iris_classification_model_2',
        python_callable=train_iris_model_2,
    )
    iris_classification_model_3 = PythonOperator(
        task_id='train_iris_classification_model_3',
        python_callable=train_iris_model_3,
    )
    iris_classification_model_1 >> iris_classification_model_2 >> iris_classification_model_3

```


Now go to Airflow Webserver and serach for `iris_classification_models` in dag search 
It will Automatically sync to the DAGs which are created by Python file.

![](https://cf-courses-data.s3.us.cloud-object-storage.appdomain.cloud/IND-GPXX0DNQEN/images/find_dag.png)
