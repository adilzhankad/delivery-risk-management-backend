# How to Run the Logistics Risk Management Project

This guide explains how to set up and start the entire backend system from scratch. The project includes a FastAPI server, a PostgreSQL database, and a machine learning service.

## Step 1: Clone the Repository

Open your terminal and download the code from GitHub:

```bash
git clone git@github.com:adilzhankad/delivery-risk-management-backend.git
cd delivery-risk-management-backend

```

## Step 2: Configure Environment Variables

You must create a file named .env in the root directory to store sensitive information. Copy and paste the following content into it:

```text
DATABASE_URL=postgresql://user:password@db:5432/logistics_risk_db
SECRET_KEY=your_random_secret_string
ALGORITHM=HS256
MAIL_USERNAME=hanagooru@gmail.com
MAIL_PASSWORD=xjlr oyzw pfks nwqv
MAIL_FROM=hanagooru@gmail.com

```

Note: The MAIL_PASSWORD is a 16-character Google App Password required for the SMTP service to send verification emails.

## Step 3: Launch the System with Docker

The project is containerized, so you do not need to install Python or PostgreSQL manually on your machine. Run this command:

```bash
docker-compose up --build -d

```

This command builds the application and starts all services in the background.

## Step 4: Update the Database Schema

If you are running the project for the first time or on a new machine, you must ensure the database tables have the columns needed for email verification and soft deletion. Run these commands in your terminal:

1. Access the database container:

```bash
docker exec -it logistics_db psql -U user -d logistics_risk_db

```

2. Run these SQL commands inside the database:

```sql
ALTER TABLE users ADD COLUMN verification_code VARCHAR(10);
ALTER TABLE users ADD COLUMN is_active BOOLEAN DEFAULT TRUE;
ALTER TABLE users ADD COLUMN is_verified BOOLEAN DEFAULT FALSE;
\q

```

## Step 5: Verify the Installation

Open your web browser and go to the interactive documentation:
[http://127.0.0.1:8000/docs](https://www.google.com/search?q=http://127.0.0.1:8000/docs)

You can now test the full user journey:

1. Create an account using the /register endpoint.
2. Check your email for a 6-digit code and verify it using /verify-email.
3. Log in at /login to get a JWT token.
4. Authorize your session in Swagger by clicking the Authorize button and entering your token.
5. Create a delivery order at /orders to see the ML risk prediction in action.

## Management Commands

* To stop the project: `docker-compose down`
* To see live error logs: `docker logs -f logistics_app`
* To restart the server after code changes: `docker-compose restart logistics_app`

