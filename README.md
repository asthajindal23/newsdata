#Project:
##Log Analysis Projet

##Project Overview:
>In this project, you will stretch your SQL database skills. You will get practice interacting with a live database both from the command line and from your code. You will explore a large database with over a million rows. And you will build and refine complex queries and use them to draw business conclusions from data.

### PreRequisites:
  1. [Python3](https://www.python.org/)
  2. [Vagrant](https://www.vagrantup.com/)
  3. [VirtualBox](https://www.virtualbox.org/)

###Project includes:
    newsdata.py

####Starting Virtual Machine:
   1.Keep project file inside your vagrant directory and run below command to start your VM
     ```
    $vagrant up
     ``` 
   2.Log in VM using:
     ```
    $vagrant ssh
     ```
   3.Change your directory using below command
      ```
    $/vagrant
      ```
   4.Use ls command to get into the project folder named  newsdata
   5.Load data using:
    ```
    psql -d news -f newsdata.sql
    ```
   6.Connect to database using:
    ```
    psql -d news
    ```
    7.Create view :
       popular :
      ```
      create view popular as selecty articles.title,count(log.path)as views from articles,log group by log.path,articles.title,articles.slug having substr(log.path,10)=articles.slug order by count(log.path) desc limit 3;
      ```
      author_des:
      ```
      create view author_des as select title,name from articles,authors where articles.author=authors.id;
      ```

      views:
      ```
      create view views as select article.title,count(log.path) as views from articles,log group by log.path,articles.title,articles.slug having substr(log.path,10)=articles.slug order by count(log.path)DESC;
      ```
      author_pop:
      ```
      create view author_pop as select author_des.name,views.views from author_des,views where author_des.title=views.title;
      ```

      total:
      ```
      create view total as select cast(time as date),count(status)as count from log group by cast(time as date);
      ```

      error:
      ```
      create view error as select cast(time as date),count(status) as count from log group by cast(time as date),status having status='404 NOT FOUND';
      ```

      perc:
      ```
      create view perc as select to_char(error.time,'MONTHDD,YYYY') as date, 0.01*error.count as errors from error,total where error.time = total.time and error.count>0.01*total.count;
      ```
    8.Run queries using:
      ```
      $ python newsdata.py
      `` 








