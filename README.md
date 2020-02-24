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
