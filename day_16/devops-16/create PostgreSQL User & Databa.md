# Day 16 - PostgreSQL User & Database Setup

## Problem

Create PostgreSQL user and database for application.

Requirements:

User:

kodekloud_sam


Password:

B4zNgHA7Ya


Database:

kodekloud_db3


Grant full permission to user.

---

## Solution

Login:

```bash
ssh peter@stdb01

Switch postgres user:

sudo -i -u postgres

Open PostgreSQL:

psql

Create user:

CREATE USER kodekloud_sam WITH PASSWORD 'B4zNgHA7Ya';

Create database:

CREATE DATABASE kodekloud_db3;

Grant permission:

GRANT ALL PRIVILEGES ON DATABASE kodekloud_db3 TO kodekloud_sam;

Verify:

\du
\l
Learning
User creation
Database creation
Permission management
PostgreSQL basics