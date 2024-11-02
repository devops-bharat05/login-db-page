# Flask MySQL Login Page

This project is a simple Flask-based login page that uses MySQL as a backend to store user details. Upon successful login, the user's name is displayed on the page, running on port 5000.

## Project Features

- **User Authentication:** Users can log in to see their information.
- **Data Storage:** User credentials are securely stored in a MySQL database.
- **Configuration:** Easy setup with environment configuration.
- **Local and Remote Access:** MySQL can be configured for remote access if needed.

## Prerequisites

1. **Operating System**: Ubuntu (tested), or other Linux-based distributions.
2. **Python**: Python 3.x installed on your machine.
3. **MySQL Server**: For storing user information.

## Installation

### Step 1: Update and Install Required Packages

```bash
sudo apt-get update
sudo apt install python3
```

### Step 2: Clone the Repository

```bash
git clone https://github.com/devops-bharat05/login-db-page/
cd login-db-page
```

### Step 3: Set Up a Virtual Environment

1. Install the `venv` package if it's not already installed:

    ```bash
    sudo apt install python3.12-venv -y
    ```

2. Create and activate a virtual environment:

    ```bash
    sudo python3 -m venv myenv
    source myenv/bin/activate
    ```

3. Install the project dependencies:

    ```bash
    pip install -r requirements.txt
    ```

    > Note: If you encounter permission errors, try running the command as root.

### Step 4: Install and Configure MySQL

1. Install MySQL Server:

    ```bash
    sudo apt install mysql-server -y
    sudo systemctl start mysql
    sudo systemctl enable mysql
    ```

2. Set the MySQL root password and configure the user:

    ```sql
    sudo mysql
    ALTER USER 'root'@'localhost' IDENTIFIED WITH mysql_native_password BY 'password';
    FLUSH PRIVILEGES;
    ```

### Step 5: Database Setup

1. Create a database and a table to store user details:

    ```sql
    CREATE DATABASE mydb;
    USE mydb;
    CREATE TABLE users (
        id INT AUTO_INCREMENT PRIMARY KEY,
        username VARCHAR(50) NOT NULL,
        password VARCHAR(255) NOT NULL
    );
    CREATE USER 'root'@'%' IDENTIFIED BY 'password';
    GRANT ALL PRIVILEGES ON mydb.* TO 'root'@'%';
    FLUSH PRIVILEGES;
    ```

2. Allow remote connections (optional, if accessing MySQL from another server):

    ```bash
    sudo vim /etc/mysql/mysql.conf.d/mysqld.cnf
    # Change bind-address to
    bind-address = 0.0.0.0
    sudo systemctl restart mysql
    ```

### Step 6: Configure the Application

In `app.py`, update the MySQL connection details to match your setup, including the host IP if you're deploying to a remote server.

```python
# Example configuration in app.py
app.config['MYSQL_DATABASE_USER'] = 'root'
app.config['MYSQL_DATABASE_PASSWORD'] = 'password'
app.config['MYSQL_DATABASE_DB'] = 'mydb'
app.config['MYSQL_DATABASE_HOST'] = '<your-ec2-hostIP>'
```

### Step 7: Run the Application

Start the Flask application:

```bash
python app.py
```

Visit `http://localhost:5000` in your browser to access the login page.

## Additional Tips

- **Security**: Remember to secure your MySQL credentials in production, possibly using environment variables.
- **Deployment**: This application can be deployed on an EC2 instance, with the necessary ports opened for remote access if required.
- **Remote Access Configuration**: If deploying on a cloud service, ensure the MySQL server is configured for remote connections only when necessary, as it may pose security risks.

## Contributing

1. Fork the repository.
2. Create a feature branch (`git checkout -b feature-branch`).
3. Commit your changes (`git commit -am 'Add new feature'`).
4. Push to the branch (`git push origin feature-branch`).
5. Create a new Pull Request.

## License

This project is licensed under the MIT License - see the LICENSE file for details.

---

This README includes installation instructions, additional configuration tips, and contribution guidelines. Let me know if you'd like more details added!
