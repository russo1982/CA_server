# CA Server
The repo has several branches with manuals on how to setup local CA server.
___
___

## EJBCA (Enterprise-Grade) CA server setup

For a full-featured CA, use EJBCA. It requires Java, a database (e.g., MariaDB), and WildFly.

#### 1. **Install Dependencies:**
```
sudo apt install openjdk-17-jdk mariadb-server ant
```

#### 2. **Download EJBCA:**
```
wget https://sourceforge.net/projects/ejbca/files/ejbca7/ejbca_7_9_0.zip
unzip ejbca_7_9_0.zip
```

#### 3. **Secure MariaDB Installation:**

   Run the security script to set up the root password and remove insecure defaults:
```
sudo mysql_secure_installation
```
- Set a strong root password.
- Answer `Y` to all prompts to remove anonymous users, disable remote root login, etc.

#### 4. **Create a Database and User for EJBCA**

   Log into MariaDB:
   ```
   sudo mysql -u root -p
   ```
   Create a dedicated database and user for EJBCA:
```
-- Create database
CREATE DATABASE ejbca CHARACTER SET utf8mb4 COLLATE utf8mb4_unicode_ci;

-- Create user (replace 'ejbca_password' with a strong password)
CREATE USER 'ejbca'@'localhost' IDENTIFIED BY 'ejbca_password';

-- Grant privileges
GRANT ALL PRIVILEGES ON ejbca.* TO 'ejbca'@'localhost';
FLUSH PRIVILEGES;
EXIT;
```

#### 5. **Configure MariaDB for EJBCA**
   Edit the MariaDB configuration file:
   ```
   sudo nano /etc/mysql/mariadb.conf.d/50-server.cnf
   ```
   Add/update the following under the `[mysqld]` section:
   ```
   [mysqld]
   character-set-server = utf8mb4
   collation-server = utf8mb4_unicode_ci
   ```
   Restart MariaDB:
   ```
   sudo systemctl restart mariadb
   ```

#### 6. **Configure EJBCA to Use MariaDB**
   6.1 **Edit EJBCA's Database Configuration:**
   Navigate to your EJBCA installation directory and edit `conf/database.properties`
   ```
nano ejbca/conf/database.properties
```
Update the file with:
```
database.name=mysql
database.url=jdbc:mysql://localhost:3306/ejbca?characterEncoding=UTF-8
database.driver=org.mariadb.jdbc.Driver
database.username=ejbca
database.password=ejbca_password
```

6.2 **Install MariaDB JDBC Driver:**
Download the MariaDB JDBC driver:
```
wget https://downloads.mariadb.com/Connectors/java/connector-j-3.3.0/mariadb-java-client-3.3.0.jar
```

Copy it to EJBCA's `lib` folder:
```
cp mariadb-java-client-3.3.0.jar ejbca/lib/
```
---



#### 7. **Build and Deploy:**
   From the EJBCA directory, run:
```
ant deploy
ant install
```
This will create the required tables in the `ejbca` database.

#### 8. **Verify the Setup**
    Check if tables were created:
   ```
   sudo mysql -u ejbca -p ejbca -e "SHOW TABLES;"
   ```
   

#### 9. **Access the Web UI:**

   Visit `http://localhost:8080/ejbca/adminweb/`.

