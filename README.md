# CloudFormation Tutorial

This is a step by step guide on how to build and provision your own s3 bucket to host your own static website

## Installation

To use CloudFormation you must has a text editor and a AWS account 
Cloudformation is a template language you can either use ```YAML``` or ```JSON```.

It will be great if you're familiar with some of the services as we will be using the CloudFormation service and S3 Service.

## Code step by step

Please set up a file it can be ```test.yaml``` or ```mybucket.yaml``` something that is relevant to the task

1. Adding the template heading

```
AWSTemplateFormatVersion: 2010-09-09
Description: "Creating an s3 bucket to host static content"
```
You will need to add this at the top of the file

2. Adding the parameter
```Parameters:
  S3BucketName: 
    Type: String
    Description: "Name of S3Bucket"
```
*Note: Parameter is a field you can add in when uploading the template in the Cloudformation Service*

3. Adding the resource

```Resources:
  S3Bucket:
    Type: AWS::S3::Bucket
    Properties:
      AccessControl: PublicRead
      WebsiteConfiguration:
        IndexDocument: index.html
        ErrorDocument: error.html
```

4. Adding the bucket policy 

```  BucketPolicy:
    Type: AWS::S3::BucketPolicy
    Properties:
      PolicyDocument:
        Id: MyPolicy
        Version: 2012-10-17
        Statement:
          - Sid: PublicReadForGetBucketObjects
            Effect: Allow
            Principal: '*'
            Action: 's3:GetObject'
            Resource: !Join 
              - ''
              - - 'arn:aws:s3:::'
                - !Ref S3Bucket
                - /*
      Bucket: !Ref S3Bucket
```

5. The final block of code will look like this:

```
AWSTemplateFormatVersion: 2010-09-09
Description: "Creating an s3 bucket to host static content"

Parameters:
  S3BucketName: 
    Type: String
    Description: "Name of S3Bucket"
    
Resources:
  S3Bucket:
    Type: AWS::S3::Bucket
    Properties:
      AccessControl: PublicRead
      WebsiteConfiguration:
        IndexDocument: index.html
        ErrorDocument: error.html
        
  BucketPolicy:
    Type: AWS::S3::BucketPolicy
    Properties:
      PolicyDocument:
        Id: MyPolicy
        Version: 2012-10-17
        Statement:
          - Sid: PublicReadForGetBucketObjects
            Effect: Allow
            Principal: '*'
            Action: 's3:GetObject'
            Resource: !Join 
              - ''
              - - 'arn:aws:s3:::'
                - !Ref S3Bucket
                - /*
      Bucket: !Ref S3Bucket
```

6. To upload the file ensure you save the file and go to the aws console cloudformation service and follow the prompts to upload template. Once stack is GREEN and has passed you can check to validate to see if your bucket is in the console, if so, you can add your index.html file and hit the endpoint to access your webpage. 
