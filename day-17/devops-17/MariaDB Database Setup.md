# Day 17 - MariaDB Database Setup

## Problem

Setup MariaDB database server and create database/user for application.

Requirements:

Database:

kodekloud_db10


User:

kodekloud_tim


Password:

B4zNgHA7Ya


Grant full permissions to user on database.

---

## Solution

### Login to DB Server

```bash
ssh peter@stdb01
Install MariaDB
sudo yum install mariadb-server -y
Start and Enable Service
sudo systemctl start mariadb

sudo systemctl enable mariadb

Check:

systemctl status mariadb
Login MariaDB
mysql
Create Database
CREATE DATABASE kodekloud_db10;
Create User
CREATE USER 'kodekloud_tim'@'localhost'
IDENTIFIED BY 'B4zNgHA7Ya';
Grant Permissions
GRANT ALL PRIVILEGES ON kodekloud_db10.*
TO 'kodekloud_tim'@'localhost';

FLUSH PRIVILEGES;
Verify Database
SHOW DATABASES;
Verify User
SELECT user FROM mysql.user;
Verify Permissions
SHOW GRANTS FOR 'kodekloud_tim'@'localhost';