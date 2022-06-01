# emr-serverless-playground

learn [EMR Serverless](https://docs.aws.amazon.com/emr/latest/EMR-Serverless-UserGuide/emr-serverless.html)

## Resource Types

- application - `arn:${Partition}:emr-serverless:${Region}:${Account}:/applications/${ApplicationId}`
- jobRun `arn:${Partition}:emr-serverless:${Region}:${Account}:/applications/${ApplicationId}/jobruns/${JobRunId}`

## CloudFormation Support

[AWS::EMRServerless::Application](https://docs.aws.amazon.com/AWSCloudFormation/latest/UserGuide/aws-resource-emrserverless-application.html)

example `AWS::EMRServerless::Application` usage in cfn

- <https://github.com/aws-samples/emr-serverless-samples/blob/main/cloudformation/emr_serverless_spark_app.yaml#L6>
- <https://github.com/aws-samples/emr-serverless-samples/blob/main/cloudformation/emr_serverless_full_deployment.yaml#L61>

## Resources

- [EMR Serverless](https://docs.aws.amazon.com/emr/latest/EMR-Serverless-UserGuide/emr-serverless.html)
- [Amazon EMR Serverless Now Generally Available – Run Big Data Applications without Managing Servers](https://aws.amazon.com/blogs/aws/amazon-emr-serverless-now-generally-available-run-big-data-applications-without-managing-servers/)
- [AWS::EMRServerless::Application](https://docs.aws.amazon.com/AWSCloudFormation/latest/UserGuide/aws-resource-emrserverless-application.html)
- [Actions, resources, and condition keys for Amazon EMR Serverless](https://docs.aws.amazon.com/service-authorization/latest/reference/list_amazonemrserverless.html)