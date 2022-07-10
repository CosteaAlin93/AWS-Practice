Time to learn a bit about Lambda Events and Lambda Functions.

1) Create 2 S3 Buckets - Source and Destination
- default settings will be fine

![image](https://user-images.githubusercontent.com/86648102/178157976-98cc4e1d-4ac2-48f6-8e42-7e8df0f3e2c9.png)

2) Create Lambda Role
- everything in AWS runs only if allowed...so we need an AWS IAM role for this lambda function for it to have access to our needed resources

![image](https://user-images.githubusercontent.com/86648102/178158092-57e2a62a-7923-4e62-961b-d805903dc1a0.png)

- for the following 2 steps, click Next.

![image](https://user-images.githubusercontent.com/86648102/178158215-c9e3c782-c4a3-4d49-a32c-a520e1a1cd65.png)

- click on the newly created role and then Add Permissions > Create inline policy

![image](https://user-images.githubusercontent.com/86648102/178158272-d827f107-6d82-45d3-b408-ce68ae3a6f6b.png)

- switch to the JSON tab, delete everything and replace with this template ( https://raw.githubusercontent.com/acantril/learn-cantrill-io-labs/master/00-aws-simple-demos/aws-lambda-s3-events/01_LABSETUP/policy/s3pixelator.json )

![image](https://user-images.githubusercontent.com/86648102/178158310-ba77df67-5a7e-4115-8b27-08cb5393e7a7.png)

- update template with your current infos (S3 buckets ARNs
- - for buckets: 

![image](https://user-images.githubusercontent.com/86648102/178158381-93eb219a-14b3-42e1-9cca-669fcd8e4d82.png)

- - for account ID:

![image](https://user-images.githubusercontent.com/86648102/178158453-5f6bd33f-5466-4863-a794-3b2401b89b88.png)

- click Review Policy (errors will appear in case something is not right
- add Policy Name and Create

![image](https://user-images.githubusercontent.com/86648102/178158607-952d946d-d4a6-4cf9-9805-a4d6fa1aa4b0.png)

3) Create the Lambda Function 

- do the settings from below while using the role we just created:

![image](https://user-images.githubusercontent.com/86648102/178158829-d2b624a6-e527-49f7-a861-16bd158dc6c8.png)

- upload the zip file needed for the lambda to run

![image](https://user-images.githubusercontent.com/86648102/178158924-760dde06-d8b0-49da-9cab-7ce08dd17ec7.png)

- download it from Adrian's repo

https://github.com/acantril/learn-cantrill-io-labs/blob/master/00-aws-simple-demos/aws-lambda-s3-events/01_LABSETUP/my-deployment-package.zip

![image](https://user-images.githubusercontent.com/86648102/178158980-2156b7f8-5af8-4ddd-807c-3e7ce4b504b5.png)


4) Configure the Lambda Function and Trigger

- in your function to the below actions and add environment variable

![image](https://user-images.githubusercontent.com/86648102/178159050-3cd06786-1cf9-4e30-95dc-6bef411adb94.png)

- - for Key, use processed_bucked and for Value, use the name of the bucket you created in the 1st step

![image](https://user-images.githubusercontent.com/86648102/178159140-44f5cbf0-4d8d-4914-b237-4c5916c5309b.png)

- some additional settings regarding Timeout

![image](https://user-images.githubusercontent.com/86648102/178159208-ecbd1512-7d41-401d-986c-d39e4bbc6ab8.png)
![image](https://user-images.githubusercontent.com/86648102/178159244-a0925959-31a2-4cd7-b20a-c6b0f77966b2.png)

-  

