# Step-by-step Implementation Guide

In this project I will demonstrate how to set up an API using AWS Lambda & Amazon API Gateway 
I am doing this to learn how APIs work & also set up the logic tier for this three tier architecture.

## Step 1 - The Lambda Function 
- Create a Lambda Function
- Condfigure the settings
- add code to retrieve user data

Lambda function will be the brain of my application, it's a service that lets me run a code without having to manage servers. its a Function as a Service. In this use case it will fetch data from a database & return it to users. A common use case in Web Applications.
You can write Lmabda Functions to do things like process data, respond to events & run automated tasks.
for example when you search for a product in an online shop, the website backend needs to query the product database,
find the relevant product & send the details back to the browser as quickly as possible.

Lambda runs the code only when needed which (no payment for idle time)
It scales automatically, from a few requests per day to thousans per second. The only requirement is to supply a code in a language supported by Lambda, in this case I'lll be using python.

<img width="1919" height="247" alt="lambda step 1" src="https://github.com/user-attachments/assets/45950f00-822c-4672-b244-46e122d8b09d" />

<img width="1233" height="821" alt="the function" src="https://github.com/user-attachments/assets/22cac507-69d1-4b95-b6ef-882fa2ec8f53" />


- Create Lambda function & Select Author from scratch
- functions name `Retrieve Data`
- Run time is the enviroment used to run the code. Python code needs a Python runtime environment to execute our Lambda Function.
- execution role defines permissions of the function. sometimes Lambda needs permissions to access other service.
We create basic lamdbda permissions to write logs to CloudWatch to troubleshoot errors. 
- For Architecture I selected x86_64, it is the physical processor/hardware somewhere in the world of AWS which runs the code.

<img width="1205" height="444" alt="lambda" src="https://github.com/user-attachments/assets/f77d607c-5387-41f6-993c-174dbbb3f550" />

Successfully created the Lambda Function.

# Function & Code
The next step is to create a Function & give it a purpose. I added a code into the function editor.

<img width="1272" height="1010" alt="dynamo code" src="https://github.com/user-attachments/assets/691edf26-cbb8-451d-a569-bda202a90a69" />



This code sets up a Lambda Function that retrieves data from a DynamoDB table.
It looks for specific data based on `UserID` & returns the data.
If there is an error, `UserID` does not exist in the database & an error message is returned.

Next I deployed the code which makes the function ready to use.
With the Lambda Function ready I need a way to access it & this is where AP
I Gateway comes in.

## Step 2 - Set Up an API Gateway

API stands for Application Programming Interface, it's a way different software systems communicate with each other.
Its like a messenger that carries requests & responses between systems. Specifically for this project, we're creating an API that will be the runner between the Users Request in the browser browser & the Lambda Function that will process these request. The API also returns the functions response.
