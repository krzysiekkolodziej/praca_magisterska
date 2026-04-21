# Stock Exchange Performance Testing Environment

This project is a containerized stock exchange simulation platform designed for performance analysis and behavioral classification. The system is built using **Django REST Framework**, **PostgreSQL**, **Redis**, and **Celery**, with **Locust** acting as the load generator.

## 1. Prerequisites
* **Docker** and **Docker Compose** installed.
* **Bash** environment (Linux, macOS, or WSL on Windows).
* Execution permissions granted to the scripts:
  ```bash
  chmod +x start_test.sh get_logs.sh
  ```
2. Test Configuration (parametry.txt)
To customize the test scenarios, modify the parametry.txt file. Each line in this file represents one independent test case.

Line Format:
```bash
number_of_users,spawn_rate,test_time,user_classes,transaction_time,request_delay,trade_container_count
```
Example:
```bash
1000,1,2h,WebsiteActiveUser WebsiteReadOnlyUser WebsiteActiveUserWtihMarketAnalize,30.0,0.6,3
```
Parameter Description:
number_of_users (e.g., 1000): Total number of simulated users to be spawned.

users_spawn_rate (e.g., 1): The number of users added per second until the total count is reached.

test_time (e.g., 2h): Duration of the test (e.g., 1h for one hour, 30m for thirty minutes).

users_classes (e.g., WebsiteActiveUser...): A space-separated list of Locust behavior classes to be used in the test.

transaction_time (e.g., 30.0): The interval (in seconds) for stock order matching and execution operations.

traffic_time_request (e.g., 0.6): The "think time" or delay (in seconds) between consecutive requests made by a single user.

number_of_trade_container (e.g., 3): The number of replicas for the celery_worker_execute_transactions container, used to test transaction processing scalability.

3. Running the Tests
To start the automated testing procedure, run the following script:

```bash
./start_test.sh
```
What does the script do?
It reads the parameters from parametry.txt.

It dynamically generates a docker-compose.generated.yml file for the specific scenario.

It builds and launches the entire container stack (Databases, Redis, API, Workers, Monitor, and Locust).

It runs the simulation for the duration defined in the parameters.

4. Retrieving Logs and Results
During the test, the system collects performance metrics (API response times, CPU/RAM utilization, SQL query times). To extract these logs from the test database, run:

```bash
./get_logs.sh
```
Test Completion:
Once the defined test_time has elapsed, the test will terminate automatically.

All collected performance logs will be saved in a file named logs.