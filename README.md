# Send an email notification if a particular instance is stopped.
# We will use Lambda service which will trigger CloudWatch events and send a message in case on of the instances stopped. 
# You will see in this example the steps how to configure it.
For that we need to create an EC2 instance, SNS topic, IAM policy, Lambda function and Cloud watch events. 
There are two options to get a notifications: 1) To get for all stopped instances 2) To get a notification for the particular instance.
We will create one instance and stop it, after couple of seconds you will get an email about it. 



1) In the beginning need to create an instance in us-east-1 region:
<img width="1063" alt="First" src="https://github.com/user-attachments/assets/6e6d39a8-018b-4654-8f35-cb1296ccca1a" />



2) Next step is to create SNS service, where you need to add your email address to get notifications in case one of instances stops: 
<img width="1214" alt="Second" src="https://github.com/user-attachments/assets/105ef23c-13f8-4407-ae8f-b4c039882351" />


 
3) After saving your email address you will get a message to confirm it:
<img width="573" alt="third" src="https://github.com/user-attachments/assets/9917ab0d-a4c3-4b96-8530-000855b587a4" />



4) After confirming, we need to create IAM policy and role then attach to Lambda function:
<img width="792" alt="forth" src="https://github.com/user-attachments/assets/a171d72e-9e2b-4e26-9fa9-de4b0bbcdad1" />



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



   
    
