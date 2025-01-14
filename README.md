# anti-obesity web app

To run the given Flask application, you will need to have several dependencies and requirements installed. Here is a detailed breakdown of the requirements:

### Python Packages
You need to install various Python packages. Here's a `requirements.txt` file that includes the necessary dependencies:


To install these dependencies, you can create a `requirements.txt` file and run:

```bash
pip install -r requirements.txt
```

### MySQL Server
You need to have MySQL server installed and running. You can install it using the following commands:

```bash
sudo apt-get update
sudo apt-get install mysql-server
```

After installing MySQL, ensure you create a user and a database:

```sql
CREATE DATABASE webappdb;
CREATE USER 'user'@'localhost' IDENTIFIED BY 'password';
GRANT ALL PRIVILEGES ON webappdb.* TO 'user'@'localhost';
FLUSH PRIVILEGES;
```

Replace `'user'` and `'password'` with your actual MySQL username and password.

### Celery and a Message Broker (RabbitMQ or Redis)
Celery requires a message broker to handle asynchronous tasks. You can use either RabbitMQ or Redis. Below are the installation steps for both:

#### RabbitMQ
```bash
sudo apt-get install rabbitmq-server
sudo systemctl enable rabbitmq-server
sudo systemctl start rabbitmq-server
```

#### Redis
```bash
sudo apt-get install redis-server
sudo systemctl enable redis-server
sudo systemctl start redis-server
```

Update your Flask app configuration to use the appropriate broker URL in `config.py` or directly in the app configuration:

```python
app.config['CELERY_BROKER_URL'] = 'redis://localhost:6379/0'  # For Redis
app.config['CELERY_RESULT_BACKEND'] = 'redis://localhost:6379/0'
# or
app.config['CELERY_BROKER_URL'] = 'amqp://localhost//'  # For RabbitMQ
app.config['CELERY_RESULT_BACKEND'] = 'rpc://'
```

### Setting Up the Database
Before running your Flask app, make sure to initialize the database:

```bash
flask db init
flask db migrate -m "Initial migration."
flask db upgrade
```

### Running the Application
Finally, you can run the application with:

```bash
python app.py
```

Make sure to run your Celery worker in another terminal:

```bash
celery -A app.celery worker --loglevel=info
```

### Summary of Steps
1. Create and activate a virtual environment.
2. Install required Python packages using `pip install -r requirements.txt`.
3. Install and configure MySQL server.
4. Install and configure RabbitMQ or Redis for Celery.
5. Set up your directory structure correctly.
6. Initialize and migrate your database.
7. Run your Flask application and Celery worker.
