# python-flask-microservices
# 🧩 Microservices Setup and Configuration

This project is a complete end-to-end microservices application composed of independent services for **User**, **Product**, **Order**, and a **Frontend** interface. It includes Docker-based orchestration, MySQL databases, and Flask-based API services.

---

##  Microservices Included

- **User Service** (`cuser-service`)
- **Product Service** (`cproduct-service`)
- **Order Service** (`corder-service`)
- **Frontend Service** (`cfrontend-app`)
- MySQL Databases for each service

---


###  Step 1: Build and Launch All Microservices

```bash
cd frontend
docker-compose -f docker-compose.deploy.yml build
docker-compose -f docker-compose.deploy.yml up -d
## step 2.
Prepare each microservice mysql database (If not created):

for service in corder-service cproduct-service cuser-service; do
  docker exec -it $service flask db init
  docker exec -it $service flask db migrate
  docker exec -it $service flask db upgrade
done
Step 3: Populate Product Database

curl -i -d "name=prod1&slug=prod1&image=product1.jpg&price=100" -X POST localhost:5002/api/product/create
curl -i -d "name=prod2&slug=prod2&image=product2.jpg&price=200" -X POST localhost:5002/api/product/create
##Step 4.
Using your workstations browser - navigate to the following URL and register:

http://localhost:5000/register
Step 5.
Back within your terminal, use a mysql client to confirm that a new user registration record was created:

mysql --host=127.0.0.1 --port=32000 --user=danops --password=danops_2022
mysql> show databases;
mysql> use user;
mysql> show tables;
mysql> select * from user;
mysql> exit

 Build Services Individually
🔹 Build User Service
cd User
docker-compose build
docker-compose up -d
docker exec -it cuser-service flask db upgrade

 Build Product Service
cd Product
docker-compose build
docker-compose up -d
docker exec -it cproduct-service flask db upgrade

Build Order Service
cd Order
docker-compose build
docker-compose up -d
docker exec -it corder-service flask db upgrade

Microservices Teardown
Perform the following steps to teardown the microservices environment:

Step 1.
Create a new Docker network and name it micro_network:

for container in cuser-service cproduct-service corder-service cproduct_dbase cfrontend-app cuser_dbase corder_dbase;
do
 docker stop $container
 docker rm $container
done
Step 2.

Remove the container volumes

for vol in frontend_orderdb_vol frontend_productdb_vol frontend_userdb_vol;
do
 docker volume rm $vol
done

Step 3.
Remove the container network

docker network rm micro_network
