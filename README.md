# aws-RDS-lambda-dynamodb
Insert and delete data in RDS and non-RDS databases on AWS using EC2 and Lambda (MySQL, DynamoDB).

I have uploaded all the steps on Hashnode. Please check them there.
https://aws-rds-awscli.hashnode.dev/insert-and-delete-in-aws-rds-and-non-rds-from-aws-ec2-and-lambda-function

Overview

This repository demonstrates:

How to create and connect to a MySQL database using AWS RDS

How to access RDS securely using an EC2 instance

How to create, insert, and delete data in MySQL

How DynamoDB works as a serverless NoSQL database

How to insert and delete items in DynamoDB using AWS Lambda

Part 1: Create MySQL Database in AWS (RDS)
Step 1: Create an RDS MySQL Instance

Go to AWS Console → RDS

Create a MySQL database

Choose instance size, username, and password

Keep the database private (recommended for security)

Step 2: Create an EC2 Instance

You cannot directly connect to a private RDS database.

So you need:

An EC2 instance in the same VPC

Install MySQL client on EC2

sudo yum update -y
sudo yum install mysql -y

Step 3: Connect EC2 to RDS

Go to RDS → Connectivity & Security

Copy:

Endpoint

Port

Connect from EC2:

mysql -h <rds-endpoint> -u <username> -p

Step 4: Create Database and Table
SHOW DATABASES;

CREATE DATABASE testdb;
USE testdb;

CREATE TABLE information (
  name VARCHAR(50),
  age INT
);

Step 5: Insert and Delete Data
INSERT INTO information VALUES ('Meghraj', 25);

DELETE FROM information WHERE name='Meghraj';

Important Notes (RDS)

RDS is a relational database

You must:

Create database

Create tables

Define columns before inserting data

SQL databases require a server (EC2 or application server)

################################# :)

Part 2: DynamoDB with AWS Lambda (Serverless)
What is DynamoDB?

Non-relational (NoSQL)

Fully serverless

No EC2 or server required

Works directly with Lambda

Step 1: Create DynamoDB Table

Go to DynamoDB → Create table

Define:

Partition Key (example: id)

Step 2: Create Lambda Function

Runtime: Python 3

DynamoDB access via boto3

import boto3
import json

def lambda_handler(event, context):
    dynamodb = boto3.resource('dynamodb')
    table = dynamodb.Table('non-rds-database')

    table.put_item(
        Item={
            'id': '1',
            'name': 'Meghraj',
            'age': 25
        }
    )

    return {
        'statusCode': 200,
        'body': json.dumps('Item inserted')
    }

Step 3: IAM Permissions

Lambda cannot access DynamoDB by default.

You must:

Go to Lambda → Configuration → Permissions

Edit the IAM role

Add:

AmazonDynamoDBFullAccess (for learning)

Or custom policy with PutItem & DeleteItem

Step 4: Delete Item from DynamoDB
table.delete_item(
    Key={
        'id': '1'
    }
)

RDS vs DynamoDB (Quick Comparison)
Feature	         RDS (MySQL)	    DynamoDB
Type	           Relational	        NoSQL
Server Required	 Yes	                No
Schema	         Fixed	            Flexible
Scaling	         Manual	             Automatic
Best For	       Structured data	  High-scale apps


Key Takeaways

SQL databases need servers to run queries

DynamoDB is fully serverless

Lambda + DynamoDB is ideal for event-driven systems

RDS is best for complex relational data



