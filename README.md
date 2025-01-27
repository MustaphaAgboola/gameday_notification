# Gameday_notification

## Introduction
 In this project, we'll build a Game Day Notification system using AWS Lambda, Amazon SNS, and EventBridge, demonstrating how to create an automated alert system.

## Prerequisites
1. Create a free AWS account at AWS Console
2. Create an account at sportsdata.io
3. Generate an API key
4. Save your API key


## Step 1: Create SNS Topic and Email Subscription

1. Create an SNS Topic:
   - Go to AWS Console and search for `SNS`
   - Navigate to "Topics" and click "Create topic"
   - Select "Standard" type
   - Set topic name as `gameday_topic`
   - Click "Create topic"

2. Set up Email Subscription:
   - Click "Create subscription"
   - Choose "Email" as protocol
   - Enter your email address as the endpoint
   - Click "Create subscription"
   - Check your email and confirm the subscription

## Step 2: Create IAM Role for Lambda

1. Create SNS Policy:
   - Go to AWS IAM > Policies > Create policy
   - Choose JSON and paste:
   ```json
   {
       "Version": "2012-10-17",
       "Statement": [
           {
               "Effect": "Allow",
               "Action": "sns:Publish",
               "Resource": "YOUR_SNS_TOPIC_ARN"
           }
       ]
   }
   ```
   - Replace `YOUR_SNS_TOPIC_ARN` with your SNS topic ARN
   - Name the policy `gameday_sns_policy`
   - Click "Create policy"

2. Create Lambda Role:
   - Go to IAM > Roles > Create role
   - Choose "AWS service" and select "Lambda"
   - Attach these policies:
     - `gameday_sns_policy`
     - `AWSLambdaBasicExecutionRole`
   - Name the role `gameday_lambda_role`
   - Click "Create role"

## Step 3: Create Lambda Function

1. Create Lambda:
   - Go to AWS Lambda > Create function
   - Choose "Author from scratch"
   - Function name: `gameday_notifications`
   - Runtime: Python 3.13
   - Select "Use an existing role" > `gameday_lambda_role`

2. Set Up Function:
   - In the Code section, paste the Python code from [gameday_notification.py](https://github.com/MustaphaAgboola/gameday_notification/blob/main/gameday_notification.py)
   - In the configuration section add environment variables:
     ```
     NBA_API_KEY = your_sportsdata_api_key
     SNS_TOPIC_ARN = your_sns_topic_arn
     ```

3. Test Function:
   - Create test event named `test1`
   - Run test
   - Check your email for game score notification

## Step 4: Create EventBridge Schedule

1. Set Up EventBridge Rule:
   - Go to Amazon EventBridge > Create rule
   - Name: `gameday_rule`
   - Select "Schedule"

2. Configure Schedule:
   - Choose "Recurring schedule"
   - Select "Cron-based schedule"
   - Set cron expression for every 2 hours
   - Disable flexible time window

3. Set Target:
   - Select AWS Lambda
   - Choose `gameday_notifications` function
   - Allow EventBridge to create new execution role
   - Click "Create schedule"

## Conclusion

We've successfully built an automated game notification system using AWS serverless architecture. This solution combines AWS Lambda for processing, SNS for notifications, and EventBridge for scheduling, creating a reliable system that delivers game updates every two hours to your email.

Key accomplishments:
- Created a serverless notification pipeline
- Integrated with Sports Data API for real-time game information
- Set up automated email delivery through SNS
- Implemented scheduled execution using EventBridge
- Built a scalable solution with minimal maintenance required

This project demonstrates how AWS serverless services can be combined to create practical, real-world applications while keeping costs low and efficiency high.


  