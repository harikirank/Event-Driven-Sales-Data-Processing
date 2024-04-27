# Event driven Sales Data Processing
## An AWS data pipeline for processing Sales Data based S3 event notifications and Step functions.

# Prerequisites
* A good understanding of AWS Services: AWS Step functions, SQS, DynamoDB, S3, Lambda.
* Good understanding of Python, and SQL.
* Experience with Step functions.
* Knowledge of AWS security best practices, including IAM (Identity and Access Management) roles and policies.

# Project Motivation
### This project aims to leverage the power of serverless technologies to build a flexible, scalable, and cost-effective data processing pipeline, enabling businesses to focus on their core competencies while AWS handles the underlying infrastructure and scaling requirements.

# Architecture Diagram
![Architecture Diagram](./Architecture_Diagram/Architecture_Diagram_Event_Driven_Sales_Data_Processing_using_AWS_Step_Functions.png?raw=true)

# AWS Step Function Diagram
![AWS Step Function Diagram](./Images_Watermarked/19 Final State Machine of Step function.png?raw=true)

# Architecture Diagram Steps
1. Clients upload files or data to Amazon Simple Storage Service (S3).
2. AWS Step Functions, a serverless orchestration service, is triggered through S3 event notification.
3. Step Functions reads the data that is uploaded to S3.
4. A Lambda Function, a serverless compute service, is invoked to perform validation on the data in the bucket.
5. If the record has no data quality issues, the processed record is stored in Amazon DynamoDB, a NoSQL database service.
6. Step Functions sends the message to Amazon Simple Queue Service (SQS) Dead Letter Queue(DLQ), a distributed queue service, if the record has data quality issues.

## Steps and Descriptions to build the pipeline
1. **Creating an Empty AWS Step Function**
   - Initial setup of an empty AWS Step Function, to be modified later.
   ![Initial Setup of AWS Step Function](./Images_Watermarked/1%20Create%20a%20sample%20aws%20step%20function%20to%20be%20modified%20later%20on.png?raw=true)
   ![Initial Setup of AWS Step Function](./Images_Watermarked/1%20Create%20an%20empty%20aws%20step%20function.png?raw=true)

2. **Creating an Empty Orders Data Table**
   - Setup of an empty orders data table in DynamoDB.
   ![Setup of Orders Data Table](./Images_Watermarked/2%20Create%20an%20empty%20orders%20data%20table.png?raw=true)

3. **Creating EventBridge Rule for S3 Trigger**
   - Setting up an EventBridge rule to trigger the step function based on the creation event in S3.
   ![EventBridge Rule Setup](./Images_Watermarked/3%20Create%20an%20event%20bridge%20rule%20for%20triggering%20step%20func%20based%20on%20the%20create%20event%20in%20s3.png?raw=true)
   ![EventBridge Rule Setup](./Images_Watermarked/3%20Create%20an%20event%20bridge%20rule%20for%20triggering%20step%20func%20based%20on%20the%20create%20event%20in%20s3%202.png?raw=true)

4. **EventBridge Rule Created**
   - Confirmation that the EventBridge rule has been successfully created.
   ![EventBridge Rule Creation Confirmation](./Images_Watermarked/4%20event%20bridge%20rule%20created.png?raw=true)

5. **Enabling EventBridge Notifications**
   - Turning on notifications so that the rule can trigger the step function when an object is created in S3.
   ![Enabling EventBridge Notifications](./Images_Watermarked/5%20turning%20on%20event%20bridge%20notifications%20so%20that%20the%20rule%20can%20trigger%20the%20step%20function%20when%20create%20object%20occurs.png?raw=true)

6. **State Machine Triggered by Event**
   - State machine triggered and data fetched from the newly created S3 object.
   ![State Machine Triggered](./Images_Watermarked/6%20state%20machine%20triggered%20and%20data%20got%20fetched%20from%20the%20s3%20object.png?raw=true)

7. **Converting String to JSON**
   - Converting the string output from the S3 get object action to JSON format.
   ![String to JSON Conversion](./Images_Watermarked/7%20convert%20the%20string%20to%20json%20from%20the%20s3%20get%20object%20output.png?raw=true)

8. **Output Data After Conversion to JSON**
   - Viewing the output data that is generated after converting to JSON.
   ![JSON Conversion Output](./Images_Watermarked/8%20output%20data%20that%20is%20generated%20after%20converting%20to%20json.png?raw=true)

9. **Adding Code in Lambda to Process Order**
   - Implementing the Lambda function to process the order data.
   ![Lambda Processing Code](./Images_Watermarked/9%20Adding%20the%20code%20in%20lambda%20to%20process%20the%20order.png?raw=true)

10. **Integrating Lambda into the Map State Machine**
    - Adding the Lambda function into the map state machine for processing multiple items.
    ![Lambda Integration into State Machine](./Images_Watermarked/10%20Adding%20the%20lambda%20into%20the%20map%20state%20machine.png?raw=true)

11. **Filtering Output from Lambda to DynamoDB**
    - Filtering the output from Lambda to put only valid data into DynamoDB.
    ![Filtering Lambda Output to DynamoDB](./Images_Watermarked/11%20Filtering%20the%20output%20from%20lamda%20to%20put%20the%20data%20into%20dynamodb.png?raw=true)

12. **Creating DynamoDB Table for Storing Valid Records**
    - Creating a DynamoDB table specifically for storing valid records.
    ![DynamoDB Table Creation for Valid Records](./Images_Watermarked/12%20Create%20a%20dynamodb%20table%20for%20storing%20valid%20records.png?raw=true)

13. **Creating a DLQ for Invalid Orders**
    - Setting up a Dead Letter Queue (DLQ) to store invalid orders.
    ![DLQ Setup for Invalid Orders](./Images_Watermarked/13%20create%20a%20dlq%20to%20store%20invalid%20orders.png?raw=true)

14. **Error Handling to Send Invalid Orders to DLQ**
    - Adding an option to send error orders to the DLQ using error handling options in the step function.
    ![Error Handling for Invalid Orders](./Images_Watermarked/14%20Adding%20an%20option%20to%20send%20the%20error%20orders%20to%20dlq%20using%20the%20error%20handling%20option.png?raw=true)

15. **Inserting Data into DynamoDB Table**
    - Inserting the data into the DynamoDB table if no error occurs during processing.
    ![Data Insertion into DynamoDB](./Images_Watermarked/15%20inserting%20the%20data%20into%20dynamodb%20table%20if%20no%20error%20occurs.png?raw=true)

16. **Passing API Parameters to DynamoDB**
    - Passing the necessary API parameters to insert data into the DynamoDB table.
    ![Passing Parameters to DynamoDB](./Images_Watermarked/16%20Passing%20the%20api%20parameters%20to%20insert%20data%20into%20dynamodb%20table.png?raw=true)

17. **DynamoDB Retrying Mechanism**
    - Handling retries in DynamoDB in case of operational issues.
    ![DynamoDB Retrying](./Images_Watermarked/17%20DynamoDB%20retrying%20in%20case%20something%20happens.png?raw=true)

18. **Configuring SQS Queue**
    - Setting up an SQS queue to handle and identify issues with the invalid data that we are getting.
    ![Configuring SQS Queue](./Images_Watermarked/18%20Configurin%20SQS%20Queue.png?raw=true)

19. **Final State Machine of Step Function**
    - Overview of the completed state machine configuration in the AWS Step Function.
    ![Final State Machine](./Images_Watermarked/19%20Final%20State%20Machine%20of%20Step%20function.png?raw=true)

20. **Execution Succeeded for Valid Data**
    - Documenting the successful execution of the step function for valid data scenarios.
    ![Execution Success](./Images_Watermarked/20%20Execution%20Succeded%20for%20valid%20data.png?raw=true)

21. **Invalid Data Sent to DLQ**
    - Handling and routing invalid data to the designated Dead Letter Queue.
    ![Invalid Data Handling](./Images_Watermarked/21%20Invalid%20data%20sent%20to%20DLQ.png?raw=true)

22. **Data Ingested into DynamoDB Table**
    - Confirmation of data successfully ingested into the DynamoDB table.
    ![Data Ingestion into DynamoDB](./Images_Watermarked/22%20Data%20Ingested%20into%20DynamoDB%20table.png?raw=true)

23. **Invalid Data Sent to DLQ Again**
    - Additional handling of invalid data instances sent to the DLQ.
    ![Repeated Invalid Data Handling](./Images_Watermarked/23%20Invalid%20Data%20sent%20to%20DLQ.png?raw=true)

# Potential Next Steps
1. **_AWS CloudWatch_**: Integrating AWS CloudWatch to monitor the performance metrics and operational health of the AWS Step Functions, Lambda functions, DynamoDB, and SQS queues involved in the project. This integration will allow for real-time monitoring and alerting based on specific thresholds or errors, which can help in quickly addressing any operational issues and optimizing resource usage.


2. **_AWS CodePipeline and AWS CodeBuild:_** Setting up a CI/CD pipeline using AWS CodePipeline integrated with AWS CodeBuild and AWS CodeDeploy to automate the deployment of updates to the Lambda functions, DynamoDB table configurations, and Step Function changes. This setup would ensure that all deployments are consistent, repeatable, and without downtime.
