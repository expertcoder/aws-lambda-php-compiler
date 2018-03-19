# Run PHP on AWS Lambda

**Get PHP running on AWS Lamdba with just a few clicks!**

The AWS Cloudformation template I created allows you to easy compile PHP
for AWS Lambda in a matter of minutes

### Approach overview

To run PHP on AWS Lambda you can simply send the php binaries along with
your source code when you create a Lambda function. You will of course need
to compile PHP to run on the Lambda environment. Fortunately AWS tell us which
AIM we can use to compile PHP which will be compatible with Lambda.

This project was inspired by this article from AWS
https://aws.amazon.com/blogs/compute/scripting-languages-for-aws-lambda-running-php-ruby-and-go/

### Steps

1. Install aws cli tool

1. run `aws configure`

1. create a SSH key in AWS (In same region as the new EC2 instance about to be created)

1. run Cloudformation `aws cloudformation deploy --template-file template.json --stack-name stackname --capabilities CAPABILITY_IAM --parameter-overrides KeyName=test`

1. Wait several minutes for the compile to complete and the file to end up in the S3 bucket

1. destroy the Cloudformation stack

### Approach Details

I decided to use Cloudformation to simply the process of spinning up an appropriate
AIM EC2 to compile PHP.

I attached a SSD volume to the EC2 instance and used it as swap
space, allowing even the free tier `t2.micro` instance to compile fairly quickly.
Its possible to compile and remain within the AWS free tier.

### Why run PHP on Lambda?

Lambda supports many great language natively, but there are still are few
circumstances when you might want to run PHP on Lambda. You might want to take
advantage of a library which has no suitable equivalent in other languages.
Or you might have a existing PHP Application, or PHP talent in your team that
might benefit from AWS Lambda.

### TODO

* Limit the S3 policy to specific bucket
* Regions and availability zones are hard coded.
* Generate a Pre-signed S3 link to user can download the compiled binaries more easily
* Print pre-signed S3 link back to the user
* Cloudformation reports as complete but compiling and other processes have not finished
* Print command back to user to destroy stack