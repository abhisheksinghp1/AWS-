# AWS-
ALL module 
AWSTemplateFormatVersion: '2010-09-09'
Description: Test bucket using Cloud Formation

Parameters:
  BucketName:
    Type: String
    Default: jhooq-test-bucket-with-cf
  AccessLogBucket:
    Type: String
    Default: my-demo-test-logging-4bb1ed77242437c9
  KmsKeyArn:
    Type: String
    Default: arn:aws:kms:eu-north-1:242396018804:key/a68e0903-c395-4e21-b247-6dd709215ded

Resources:

  MainBucket:
    Type: AWS::S3::Bucket
    Properties:
      BucketName: !Ref BucketName

      ## Setup encryption - AES256
      #BucketEncryption:
      #  ServerSideEncryptionConfiguration:
      #    - ServerSideEncryptionByDefault:
      #        SSEAlgorithm: AES256

      ## Setup encryption - KMS Key
      BucketEncryption:
        ServerSideEncryptionConfiguration:
          - ServerSideEncryptionByDefault:
              SSEAlgorithm: aws:kms
              KMSMasterKeyID: !Ref KmsKeyArn

      ## Enable versioning
      VersioningConfiguration:
        Status: Enabled

      ## Block public access
      PublicAccessBlockConfiguration:
        BlockPublicAcls: true
        BlockPublicPolicy: true
        IgnorePublicAcls: true
        RestrictPublicBuckets: true

      ## Setup Log bucket
      LoggingConfiguration:
        DestinationBucketName: !Ref AccessLogBucket

Outputs:
  MainBucketName:
    Description: Name of the main bucket
    Value: !Ref MainBucket
