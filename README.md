# ☕️ Serverless Daily Sales Report for Café Business

## 🎯 Project Objective
The goal of this project was to implement a serverless architecture using AWS services that automatically generates and delivers a daily sales analysis report for a café business. This solution was designed to be secure, scalable, and cost-effective, freeing the business from the burden of manual report generation while supporting future growth with minimal operational overhead.

--

## ⚙️ How It Works
This system uses AWS Lambda functions to extract and process data from an RDS database. It then formats the sales data into a readable report and sends it through Amazon SNS to the café owners every morning. The whole process is triggered automatically using Amazon EventBridge on a fixed schedule, ensuring zero manual intervention.

📸 [Add screenshot of architecture diagram showing Lambda, RDS, SNS, EventBridge]

## 📁 Task 1: Preparing Lambda Code Packages
🔸 I downloaded and extracted two pre-built Lambda code packages: one for data extraction (salesAnalysisReportDataExtractor.zip) and one for report generation (salesAnalysisReport.zip). These packages were provided, so no code development was required—just deployment and configuration.

📸 [Add screenshot of unzipped files or S3 upload location, if applicable]

🛡️ 2. Creating the Data Extraction Lambda Function
🔸 I created a Lambda function named salesAnalysisReportDataExtractor to extract sales data from an Amazon RDS instance hosted within a VPC. For secure communication, I created a security group called LambdaSG that allows outbound traffic. The existing database security group was updated to allow inbound MySQL/Aurora traffic from LambdaSG only.

🔸 I deployed the Lambda function using the Python 3.11 runtime, configured it to run in Private Subnet 1 and 2, and connected it to the LambdaSG security group. The memory was set to 128 MB, with a 30-second timeout, and the handler was defined as salesAnalysisReportDataExtractor.lambda_handler.

📸 [Add screenshot of Lambda function configuration and VPC settings]

📨 3. Creating the Report Generation Lambda Function
🔸 I deployed a second Lambda function named salesAnalysisReport, which is responsible for generating and emailing the formatted report. It was configured with the same runtime, memory, and timeout settings. The function was granted permissions via the salesAnalysisReportRole.

🔸 I also set an environment variable named topicARN inside the Lambda function, which holds the Amazon SNS topic ARN used for report delivery.

📸 [Add screenshot of environment variables and IAM role permissions]

📣 4. Setting Up Amazon SNS for Email Delivery
🔸 I created an SNS topic named SalesReportTopic and used its ARN to connect with the report generation Lambda function. This allows the Lambda function to send the report via email using the SNS service.

📸 [Add screenshot of SNS topic details]

📬 5. Subscribing an Email Address to the Topic
🔸 To receive the report, I subscribed an accessible email address to the SalesReportTopic. The subscription was confirmed via the recipient’s inbox to complete the setup.

📸 [Add screenshot of email subscription confirmation message or SNS subscriptions tab]

🧪 6. Testing the Complete Setup
🔸 I tested the system by creating a test event within the AWS Lambda console and triggering the salesAnalysisReport function. The report was successfully generated and delivered to the subscribed email address.

🔸 In case of any issues, I used CloudWatch Logs to verify output and troubleshoot common errors like incorrect security group rules, timeouts, or handler name mismatches.

📸 [Add screenshot of test event and CloudWatch logs showing success]

⏰ 7. Automating with Amazon EventBridge
🔸 I created a scheduled rule in Amazon EventBridge to trigger the report generation Lambda function daily. This was done using a CRON expression that runs the function every morning at a specific time in UTC.

🔸 The rule was linked to the Lambda using the existing mySchedulerRole, allowing EventBridge to invoke the function automatically.

📸 [Add screenshot of EventBridge rule configuration]

💡 Why This Solution Works
✅ The solution scales automatically with the business. As data grows or if multiple cafés adopt the same setup, there’s no need to change the architecture—Lambda and SNS will handle it without downtime.

✅ It’s cost-effective because you only pay for what you use. There's no need to provision or manage servers, and no cost when the system is idle.

✅ The architecture is highly secure. By using private subnets, IAM roles, and tightly controlled security groups, data access is locked down while maintaining functionality.

✅ It’s fully automated. From data extraction to email delivery, the system runs daily without any human involvement, reducing manual workload and eliminating errors.

✅ The system is extensible. In the future, reports can be pushed to dashboards, archived in S3, or even sent as SMS—all without major changes to the core setup.

📊 Project Outcome
Frank and Martha, the café owners, now receive a clear and accurate sales report via email every day. This empowers them to make faster and better-informed decisions about inventory, pricing, and promotions. The setup has saved them time, reduced the need for manual reporting, and added professionalism to their business operations.

