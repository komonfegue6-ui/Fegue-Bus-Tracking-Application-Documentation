# Fegue Bus Tracking Application

## Overview
A real-time bus tracking system using AWS serverless services.

## Table of Contents
- [Project Overview](#project-overview)
- [System Architecture](#system-architecture)
- [Setup and Configuration](#setup-and-configuration)
- [Testing and Validation](#testing-and-validation)
- [Deployment and Scaling](#deployment-and-scaling)
- [Maintenance and Updates](#maintenance-and-updates)
- [Future Work](#future-work)

## Project Overview
- **Title**: Real-Time Bus Tracking Application
- **Description**: A real-time bus tracking system using AWS serverless services.
- **Target Audience**: Developers, potential collaborators, and future maintainers.

## System Architecture
- **Architecture Diagram**: [Link to Lucidchart Diagram](Fegue.png)
- **Components**:
  - **Bus Device (Simulated)**: Sends location data via MQTT to AWS IoT Core.
  - **AWS IoT Core**: Receives location data and triggers a Lambda function.
  - **AWS Lambda**: Processes incoming data and stores it in DynamoDB.
  - **Amazon DynamoDB**: Stores the latest bus locations.
  - **Amazon API Gateway**: Exposes a REST API to fetch bus locations.
  - **Frontend (HTML/JavaScript)**: Fetches bus locations from API Gateway and displays them on a map.

## Setup and Configuration
### AWS Account Setup
1. Create an AWS account.
2. Enable the AWS Free Tier.

### IAM Roles and Policies
1. Create an IAM user with programmatic access.
2. Attach policies to allow access to AWS IoT Core, Lambda, DynamoDB, API Gateway, and Amazon Location Service.

### AWS IoT Core Configuration
1. Create a thing for the bus device.
2. Set up an MQTT topic (e.g., `bus/locations`).
3. Create a rule to trigger a Lambda function.

### AWS Lambda Function
1. Create a Lambda function (e.g., `processBusLocation`).
2. Use the following code snippet:
   ```python
   import json
   import boto3

   dynamodb = boto3.resource('dynamodb')
   table = dynamodb.Table('BusLocations')

   def lambda_handler(event, context):
       bus_id = event['bus_id']
       latitude = event['latitude']
       longitude = event['longitude']
       table.put_item(
           Item={
               'bus_id': bus_id,
               'location': {
                   'latitude': latitude,
                   'longitude': longitude
               }
           }
       )
       return {
           'statusCode': 200,
           'body': json.dumps('Location updated')
       }
