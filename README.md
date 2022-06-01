# emr-serverless-playground

learn [EMR Serverless](https://docs.aws.amazon.com/emr/latest/EMR-Serverless-UserGuide/emr-serverless.html)


- [Type](https://docs.aws.amazon.com/AWSCloudFormation/latest/UserGuide/aws-resource-emrserverless-application.html#cfn-emrserverless-application-type) - Spark or HIVE
- [ReleaseLabel](https://docs.aws.amazon.com/AWSCloudFormation/latest/UserGuide/aws-resource-emrserverless-application.html#cfn-emrserverless-application-releaselabel) - EMR release version associated with the application.  see [Release versions](https://docs.aws.amazon.com/emr/latest/EMR-Serverless-UserGuide/release-versions.html)

## general flow

1. create application
1. start application
   - > your application is setup to start with pre-initialized capacity of 1 Spark driver and 1 Spark executor. Your application is by default configured to start when jobs are submitted and stop when the application is idle for more than 15 minutes. 
1. submit application jobs  

## Resource Types

- application - `arn:${Partition}:emr-serverless:${Region}:${Account}:/applications/${ApplicationId}`
- jobRun `arn:${Partition}:emr-serverless:${Region}:${Account}:/applications/${ApplicationId}/jobruns/${JobRunId}`

## AWS CLI Support

```sh
# create application
aws emr-serverless create-application \
  --type HIVE \
  --name serverless-demo \
  --release-label "emr-6.6.0" \
  --initial-capacity '{
        "DRIVER": {
            "workerCount": 1,
            "resourceConfiguration": {
                "cpu": "2vCPU",
                "memory": "4GB",
                "disk": "30gb"
            }
        },
        "TEZ_TASK": {
            "workerCount": 10,
            "resourceConfiguration": {
                "cpu": "4vCPU",
                "memory": "8GB",
                "disk": "30gb"
            }
        }
    }' \
    --maximum-capacity '{
        "cpu": "400vCPU",
        "memory": "1024GB",
        "disk": "1000GB"
    }'

# start application
# your application is setup to start with pre-initialized capacity of 1 Spark driver and 1 Spark executor. Your application is by default configured to start when jobs are submitted and stop when the application is idle for more than 15 minutes.
aws emr-serverless start-application \
    --application-id $APPLICATION_ID

# submit hive job
aws emr-serverless start-job-run \
    --application-id $APPLICATION_ID \
    --execution-role-arn $JOB_ROLE_ARN \
    --job-driver '{
        "hive": {
            "initQueryFile": "s3://'${S3_BUCKET}'/code/hive/create_table.sql",
            "query": "s3://'${S3_BUCKET}'/code/hive/extreme_weather.sql",
            "parameters": "--hiveconf hive.exec.scratchdir=s3://'${S3_BUCKET}'/hive/scratch --hiveconf hive.metastore.warehouse.dir=s3://'${S3_BUCKET}'/hive/warehouse"
        }
    }' \
    --configuration-overrides '{
        "applicationConfiguration": [
            {
                "classification": "hive-site",
                "properties": {
                    "hive.driver.cores": "2",
                    "hive.driver.memory": "4g",
                    "hive.tez.container.size": "8192",
                    "hive.tez.cpu.vcores": "4"
                }
            }
        ],
        "monitoringConfiguration": {
            "s3MonitoringConfiguration": {
                "logUri": "s3://'${S3_BUCKET}'/hive-logs/"
            }
        }
    }'
```

## CloudFormation Support

[AWS::EMRServerless::Application](https://docs.aws.amazon.com/AWSCloudFormation/latest/UserGuide/aws-resource-emrserverless-application.html)

example `AWS::EMRServerless::Application` usage in cfn

- <https://github.com/aws-samples/emr-serverless-samples/blob/main/cloudformation/emr_serverless_spark_app.yaml#L6>
- <https://github.com/aws-samples/emr-serverless-samples/blob/main/cloudformation/emr_serverless_full_deployment.yaml#L61>

## Questions

- are EC2 instances created and visible in customer EC2 console?  or are they hidden?

## Resources

- [EMR Serverless](https://docs.aws.amazon.com/emr/latest/EMR-Serverless-UserGuide/emr-serverless.html)
- [Amazon EMR Serverless Now Generally Available – Run Big Data Applications without Managing Servers](https://aws.amazon.com/blogs/aws/amazon-emr-serverless-now-generally-available-run-big-data-applications-without-managing-servers/)
- [aws-samples/emr-serverless-samples](https://github.com/aws-samples/emr-serverless-samples)
- [AWS::EMRServerless::Application](https://docs.aws.amazon.com/AWSCloudFormation/latest/UserGuide/aws-resource-emrserverless-application.html)
- [Actions, resources, and condition keys for Amazon EMR Serverless](https://docs.aws.amazon.com/service-authorization/latest/reference/list_amazonemrserverless.html)