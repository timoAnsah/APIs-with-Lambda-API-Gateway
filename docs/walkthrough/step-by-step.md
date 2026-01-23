# Step-by-step Implementation Guide

In this project I will demonstrate how to set up an API using AWS Lambda & Amazon API Gateway 
I am doing this to learn how APIs work & also set up the logic tier for this three tier architecture.

## Step 1 - The Lambda Function 
- Create a Lambda Function
- Condfigure the settings
- add code to retrieve user data

Lambda function will be the brain of my application, it will fetch data from a database & return it to users. A common use case in Web Applications
for example when you search for a product in an online shop, the website backend needs to query the product database,
find the relevant product & send the details back to the browser as quickly as possible.
AWS Lambda lets me run a code without needing to manage any computers/servers. Lambda runs the code only when needed which (no payment for idle time)
It scales automatically, from a few requests per day to thousans per second. The only requirement is to supply a code in a language supported by Lambda, in this case I'lll be using python.

<img width="1919" height="247" alt="lambda step 1" src="https://github.com/user-attachments/assets/45950f00-822c-4672-b244-46e122d8b09d" />
