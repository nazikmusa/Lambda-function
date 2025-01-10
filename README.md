# Send an email notification if a particular instance is stopped.
## We will use Lambda service which will trigger CloudWatch events and send a message in case one of the instances stopped. 
## You will see in this example the steps how to configure it.
### For that we need to create an EC2 instance, SNS topic, IAM policy, Lambda function and Cloud watch events. 
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

<img width="825" alt="fifth" src="https://github.com/user-attachments/assets/41ba0b87-26a6-48bf-993a-77b3c88773be" />


5) Next step is to create a Lambda function and attach recently created ec2-stop role:
<img width="795" alt="sixth" src="https://github.com/user-attachments/assets/c82c4234-7bac-48eb-b0db-48058fed3bfc" />


Click ec2-stop Lambda function, scroll down and copy paste these code. Go back to SNS page, copy and paste "ARN", that's for 
to get a notification in case your instance stops:

import boto3   
client = boto3.client('sns')  
def   lambda_handler(event, context):   
      Instance_id = event['detail']['instance-id'] 
      topic_arn = 'arn_of_sns_alerts'  
      message = 'your server ' + Instance_id + 'is down' 
      client.publish(TopicArn=topic_arn,Message=message)


![Screen Shot 2025-01-09 at 12 24 00 PM](https://github.com/user-attachments/assets/3c02b563-5045-46ef-82ee-ed93bf2cb06a)

It should to look like on screenshot. 
Next step is go to the SNS > ec2-stop > copy ARN and paste in topic_arn = '  '
In my case it looks like this


![Screen Shot 2025-01-09 at 12 26 23 PM](https://github.com/user-attachments/assets/eec6939c-eed3-4bea-a707-4c2fcbb135b9)

This is Python code, if you would like to read and know more about Boto3, you can go to 
https://boto3.amazonaws.com/v1/documentation/api/latest/reference/services/sns.html#sns

After adding SNS arn > click Deploy > after Add trigger > select CloudWatch Events > Create a new role > give a meaningful name ec2-stop > 
click Event pattern(to get notification as soon as an instance stops) > EC2 > EC2 instance state-change notification > Detail > Instances and State > stopped > get ec2-instance ID and add it >  click Add in the end

Whenever event happens it will trigger ec2-stop Lambda function
Now we can go to Ec2-instance and stop it, after couple of seconds, when instance stops it will trigger email notification for me
We have received a notification. 

6) Scroll up, click the add trigger function and add CloudWatch Event > Create a new rule > Rule name > Event pattern > EC2 > EC2 instance
   state-change notification > Detail > State > Instance > stopped > Add instance ID > click Add, to this ouput:
<img width="812" alt="seventh" src="https://github.com/user-attachments/assets/ef69ba3d-3895-4057-bae8-0d151afe8726" />




7) Go back to EC2-instance, stop the instance, then you should to get a notification in your email that intance was stopped:
   
<img width="812" alt="eight" src="https://github.com/user-attachments/assets/40fb3580-36a9-4502-ba81-48ec5491878f" />

8) In the end you can go to CloudWatch > open Log groups > /aws/lambda/SNS > and see all the Logs of your instance
![Screen Shot 2025-01-09 at 12 49 56 PM](https://github.com/user-attachments/assets/af8d0fae-91cc-45d9-b592-f6bce98d458e)
![Screen Shot 2025-01-09 at 12 50 29 PM](https://github.com/user-attachments/assets/8cb08a11-ae14-4a10-a5ff-ea92e43f7851)

   


   
    
