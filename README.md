# python_etl_pipeline




## [Getting started with Apache Airflow](https://towardsdatascience.com/getting-started-with-apache-airflow-df1aa77d7b1b)

### Use airflow to author workflows as directed acyclic graphs (DAGs) of tasks.

What is Dag?
> A directed acyclic graph (DAG), is a finite directed graph with no directed cycles. That is, it consists of finitely many vertices and edges, with each edge directed from one vertex to another, such that there is no way to start at any vertex v and follow a consistently-directed sequence of edges that eventually loops back to v again. Equivalently, a DAG is a directed graph that has a topological ordering, a sequence of the vertices such that every edge is directed from earlier to later in the sequence.

What is DagRun?
> A DagRun is the instance of a DAG that will run at a time. When it runs, all task inside it will be executed.
![img0](https://miro.medium.com/max/1284/1*_mhyNeLS3aiZPJB7TZ4W-g.png)

--

opr_hello >> opr_greet >> opr_sleep >> opr_respond
> ![img0](https://miro.medium.com/max/1676/1*7VL-B7vJFjSwt_TL9kxuBQ.png)

--

opr_hello >> opr_greet >> opr_sleep << opr_respond (把 task_3 & task_4 中間箭號方向相反)
> ![img1](https://miro.medium.com/max/1880/1*UdBcds6vp1BjqCGzfZzoeA.png)


--

### So, I am going to create a file with name, my_simple_dag.py
```
import datetime as dt

from airflow import DAG
from airflow.operators.bash_operator import BashOperator
from airflow.operators.python_operator import PythonOperator


def greet():
    print('Writing in file')
    with open('path/to/file/greet.txt', 'a+', encoding='utf8') as f:
        now = dt.datetime.now()
        t = now.strftime("%Y-%m-%d %H:%M")
        f.write(str(t) + '\n')
    return 'Greeted'
def respond():
    return 'Greet Responded Again'
```

### define default_args and create a DAG instance.
```
default_args = {
    'owner': 'airflow',
    'start_date': dt.datetime(2018, 9, 24, 10, 00, 00),
    'concurrency': 1,
    'retries': 0
}
```


### If you already have implemented multiprocessing in your Python then you should feel like home here.
```
with DAG('my_simple_dag',
         default_args=default_args,
         schedule_interval='*/10 * * * *',
         ) as dag:
    opr_hello = BashOperator(task_id='say_Hi',
                             bash_command='echo "Hi!!"')

    opr_greet = PythonOperator(task_id='greet',
                               python_callable=greet)
    opr_sleep = BashOperator(task_id='sleep_me',
                             bash_command='sleep 5')

    opr_respond = PythonOperator(task_id='respond',
                                 python_callable=respond)
opr_hello >> opr_greet >> opr_sleep >> opr_respond
```

You can avoid Backfilling in two ways: You set start_date of the future or set catchup = False in DAG instance. For instance, you can do something like below:

with DAG('my_simple_dag',
         catchup=False,
         default_args=default_args,
         schedule_interval='*/10 * * * *',
         # schedule_interval=None,
         ) as dag:
         

