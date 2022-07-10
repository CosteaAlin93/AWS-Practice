## Time to learn a bit about Lambda Events and Lambda Functions.

### Part of Adrian Cantrill aws-simple-demos

![image](https://user-images.githubusercontent.com/86648102/178159898-d9a07001-bca1-4e38-9705-09bdddf1ee64.png)


<details><summary> 1) Create 2 S3 Buckets - Source and Destination </summary>
- default settings will be fine

![image](https://user-images.githubusercontent.com/86648102/178157976-98cc4e1d-4ac2-48f6-8e42-7e8df0f3e2c9.png)

</details>

<details><summary> 2) Create Lambda Role </summary>
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

</details>

<details><summary> 3) Create the Lambda Function </summary>

- do the settings from below while using the role we just created:

![image](https://user-images.githubusercontent.com/86648102/178158829-d2b624a6-e527-49f7-a861-16bd158dc6c8.png)

- upload the zip file needed for the lambda to run

![image](https://user-images.githubusercontent.com/86648102/178158924-760dde06-d8b0-49da-9cab-7ce08dd17ec7.png)

- download it from Adrian's repo

https://github.com/acantril/learn-cantrill-io-labs/blob/master/00-aws-simple-demos/aws-lambda-s3-events/01_LABSETUP/my-deployment-package.zip

![image](https://user-images.githubusercontent.com/86648102/178158980-2156b7f8-5af8-4ddd-807c-3e7ce4b504b5.png)

</details>

<details><summary> 4) Configure the Lambda Function and Trigger </summary>

- in your function to the below actions and add environment variable

![image](https://user-images.githubusercontent.com/86648102/178159050-3cd06786-1cf9-4e30-95dc-6bef411adb94.png)

- - for Key, use processed_bucket and for Value, use the name of the bucket you created in the 1st step

![image](https://user-images.githubusercontent.com/86648102/178159436-d538bd6e-ea26-4d70-a6e9-0a1f04170e65.png)

- some additional settings regarding Timeout

![image](https://user-images.githubusercontent.com/86648102/178159208-ecbd1512-7d41-401d-986c-d39e4bbc6ab8.png)
![image](https://user-images.githubusercontent.com/86648102/178159244-a0925959-31a2-4cd7-b20a-c6b0f77966b2.png)

- time to add the trigger of this function

![image](https://user-images.githubusercontent.com/86648102/178159338-26c3ebaf-a969-42f1-851d-f720a16b508f.png)

Double-check all is set properly!

</details>

<details><summary> 5) Test and Monitor </summary>

- Upload a picture to the source bucket

![image](https://user-images.githubusercontent.com/86648102/178159668-248ad99d-9b76-4fb2-aef1-efd2b5590984.png)

- Move to the CloudWatch log groups and notice a log group being created for the lambda

![image](https://user-images.githubusercontent.com/86648102/178159788-12315e02-9a2c-4dc0-8ccb-9a7b824b5282.png)
![image](https://user-images.githubusercontent.com/86648102/178159865-1751fb83-f6e8-44c6-bf3e-3b9b3da101b5.png)

- Time to check what was created in the Destination bucket:

![image](https://user-images.githubusercontent.com/86648102/178159880-c3e2e373-6753-4d2c-b07a-69522e70c27b.png)

... 5 pixelated images of the original one from the Source bucket.

</details>


