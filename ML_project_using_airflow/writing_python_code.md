Writing Python Code

In this section,we will show you how to use Python code with Airflow, a powerful workflow management platform. You’ll learn how to create your own Dags (Directed Acyclic Graphs) and perform task monitoring and other operations. By the end, you’ll have the knowledge to effectively utilize Airflow to streamline your workflows and boost productivity.

Write a below code into that file `iris_classification.py`


Importing required libraries:-
  ```python
  from datetime import datetime
  from airflow import DAG
  from airflow.operators.python_operator import PythonOperator
  import pandas as pd
  import math
  import numpy as np
  ```

Create a function for reading csv file using pandas
Note: The xcom_push function is used in Apache Airflow to push a value to XCom, 
which is a feature that allows data sharing between tasks in a workflow. 
The xcom_push function is called on the task instance (context[‘ti’]) to store a 
value with a specified key in the XCom storage.

```python
def train_iris_model_1(**context):
    # Your code for training and evaluating the Iris model with KNN algorithm (DAG 1)
    iris_data = pd.read_csv('https://archive.ics.uci.edu/ml/machine-learning-databases/iris/iris.data', header=None, names=['sepal_length', 'sepal_width', 'petal_length', 'petal_width', 'class'])
    iris_data['class'] = iris_data['class'].astype('category').cat.codes
    # convert into Dictionary for passing into another function using xcom_push method
    df_dict = iris_data.to_dict(orient='records')
    # Push this df_dict as an iris using xcom_push method to access in train_iris_model2 function
    context['ti'].xcom_push(key='iris', value=df_dict)
    pass
```

* Create a function for splitting a dataframe into traning data and test data

``` python
def train_iris_model_2(**context):
    # Your code for training and evaluating the Iris model with KNN algorithm (DAG 2)
    # xcom_pull will help you to get df_dict variable from train_iris_model_1 function
    iris_dict = context['ti'].xcom_pull(key='iris')
    iris_data= pd.DataFrame(iris_dict)
    # Split train test data
    X = iris_data.iloc[:, :-1].values
    y = iris_data.iloc[:, -1].values
    split = int(0.8 * len(iris_data))
    X_train, y_train = X[:split], y[:split]
    X_test, y_test = X[split:], y[split:]
    # Convert into list
    X_train=X_train.tolist()
    y_train=y_train.tolist()
    X_test=X_test.tolist()
    y_test=y_test.tolist()
    # convert into Dictionary for passing into another function using xcom_push method
    X_train={'X_train': X_train}
    y_train={'y_train': y_train}
    X_test={'X_test': X_test}
    y_test={'y_test': y_test}
    # Push variables using xcom_push method to access in train_iris_model_3 function
    context['ti'].xcom_push(key='X_train', value=X_train)
    context['ti'].xcom_push(key='y_train', value=y_train)
    context['ti'].xcom_push(key='X_test', value=X_test)
    context['ti'].xcom_push(key='y_test', value=y_test)
    pass

```

* Build a KNN classification model and Calculate accuracy

```python

def train_iris_model_3(**context):
    # Your code for training and evaluating the Iris model with KNN algorithm (DAG 3)
    X_train = context['ti'].xcom_pull(key='X_train')
    y_train = context['ti'].xcom_pull(key='y_train')
    X_test = context['ti'].xcom_pull(key='X_test')
    y_test = context['ti'].xcom_pull(key='y_test')
    # Convert Dict into numpy array to apply KNN Algorithm
    X_train = np.array(X_train['X_train'])
    y_train = np.array(y_train['y_train'])
    X_test = np.array(X_test['X_test'])
    y_test = np.array(y_test['y_test'])
    predictions = []
    # KNN Algorithm
    for i in range(len(X_test)):
        distances = []
        for j in range(len(X_train)):
            dist = math.sqrt(sum([(a - b)**2 for a, b in zip(X_test[i], X_train[j])]))
            distances.append((dist, y_train[j]))
        distances.sort(key=lambda x: x[0])
        neighbors = distances[:3]
        classes = [neighbor[1] for neighbor in neighbors]
        # Predict Classes
        prediction = max(set(classes), key=classes.count)
        predictions.append(prediction)
    # Calculate the Accuracy
    accuracy = sum([1 for i in range(len(y_test)) if y_test[i] == predictions[i]]) / float(len(y_test))
    print(f"Accuracy: {accuracy}")

```
