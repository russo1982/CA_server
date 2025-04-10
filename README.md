# CA Server
The repo has several branches with manuals on how to setup local CA server.
___
___

## EJBCA (Enterprise-Grade) CA server setup

For a full-featured CA, use EJBCA. It requires Java, a database (e.g., MariaDB), and WildFly.

1. **Install Dependencies:**
```
sudo apt install openjdk-17-jdk mariadb-server ant
```

2. **Download EJBCA:**
```
wget https://sourceforge.net/projects/ejbca/files/ejbca7/ejbca_7_9_0.zip
unzip ejbca_7_9_0.zip
```

3. **Configure Database:**

   Edit `conf/database.properties` and set up **MariaDB**.

5. **Build and Deploy:**
```
ant deploy
ant install
```

5. **Access the Web UI:**

   Visit `http://localhost:8080/ejbca/adminweb/`.

