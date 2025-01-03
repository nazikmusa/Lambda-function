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

