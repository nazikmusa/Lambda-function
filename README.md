# Lambda-function
1) In the beginning need to create an instance in us-east-1 region:
![Screen Shot 2025-01-02 at 4 57 58 PM](https://github.com/user-attachments/assets/9d7edbe3-f2f8-48c2-8cd8-5c461d3ead4f)


2) Next step is to create SNS service, where you need to add your email address to get notifications in case one of instances stops: 
![Screen Shot 2025-01-02 at 4 58 19 PM](https://github.com/user-attachments/assets/2a15f092-9738-44a8-99f2-7db9b04a78da)

 
3) After saving your email address you will get a message to confirm it:
![Screen Shot 2025-01-02 at 4 58 39 PM](https://github.com/user-attachments/assets/9f914cad-ed02-4906-9057-874afbbde557)


4) After confirming, we need to create IAM policy and role then attach to Lambda function:
![Screen Shot 2025-01-02 at 11 50 25 PM](https://github.com/user-attachments/assets/8938694b-d1f4-4a79-893b-dc92afe34f52)


![Screen Shot 2025-01-02 at 11 51 24 PM](https://github.com/user-attachments/assets/457e4c9d-bbe7-4181-a386-333e63282265)


5) Next step is to create a Lambda function and attach recently created ec2-stop role:
 ![Screen Shot 2025-01-02 at 11 57 05 PM](https://github.com/user-attachments/assets/23888967-ec0b-4040-8b6c-88c51cf6fdfa)

Click ec2-stop Lambda function, scroll down and copy paste these code. Go back to SNS page, copy and paste "ARN", that's for 
to get a notification in case your instance stops:

import boto3
client = boto3.client('sns')

def lambda_handler(event, context):
    Instance_id = event['detail']['instance-id']
    topic_arn = 'arn_of_sns_alerts'
    message = 'your server ' + Instance_id + 'is down'
    client.publish(TopicArn=topic_arn,Message=message)


6) Scroll up, click the add trigger function and add CloudWatch Event > Create a new rule > Rule name > Event pattern > EC2 > EC2 instance
   state-change notification > Detail > State > Instance > stopped > Add instance ID > click Add, to this ouput:


![Screen Shot 2025-01-03 at 1 02 31 AM](https://github.com/user-attachments/assets/993ea484-7d94-4b1e-9951-742680f269e2)


7) Go back to EC2-instance, stop the instance, then you should to get a notification in your email that intance was stopped:
   
![Screen Shot 2025-01-03 at 1 13 56 AM](https://github.com/user-attachments/assets/c85d08a0-a80e-4c8f-bcf6-78cad0a51699)



   
    
