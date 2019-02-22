rabbitmq-plugins enable rabbitmq_management
sudo service rabbitmq-server start

sudo rabbitmqctl add_user rabbitmq_airflow_user rabbitmq_airflow_user
sudo rabbitmqctl set_user_tags rabbitmq_airflow_user administrator
sudo rabbitmqctl set_permissions -p / rabbitmq_airflow_user ".*" ".*" ".*"
sudo rabbitmqctl add_vhost rabbitmq_airflow_vhost



ps aux|grep rabbit|awk '{print $2}'
sudo kill -9 11745

rabbitmqctl add_user openstack_rabbit_user openstack_rabbit_password; rabbitmqctl set_permissions -p / openstack_rabbit_user "." "." ".*" ; rabbitmqctl set_user_tags openstack_rabbit_user administrator;


sudo kill `ps -ef | grep airflow|awk '{print $2}'`
sudo kill `ps -ef | grep rabbit|awk '{print $2}'`
sudo service postgresql restart


# 클러스터파트
## psql 설치
sudo apt-get install postgresql-client
## remote connection test
psql -h 13.124.213.43 -U airflow
psql -h 13.124.213.43 -U airflow -p 5432
psql -h ip-172-19-32-248 -U airflow   <<이게 왜 되지>>


sudo apt-get install postgresql-client





### 이렇게 해주면 다른 클러스터의 worker도 사용 가능

broker_url = amqp://rabbitmq_airflow_user:rabbitmq_airflow_user@ip-172-19-32-248:5672/rabbitmq_airflow_vhost

# Another key Celery setting
# celery_result_backend = db+mysql://airflow:airflow@localhost:3306/airflow
celery_result_backend = amqp://rabbitmq_airflow_user:rabbitmq_airflow_user@ip-172-19-32-248:5672/rabbitmq_airflow_vhost





# celery flower 쓰려면
pip3 install flower






If you are talking about self-healing workers, consider runsv or something equivalent.
