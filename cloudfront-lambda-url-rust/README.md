# Amazon CloudFront to AWS Lambda URLs

This pattern creates an Amazon CloudFront distribution on front of an AWS Lambda function URL.

Learn more about this pattern at [Serverless Land Patterns](https://serverlessland.com/patterns/cloudfront-lambda-sam-rust).

Important: this application uses various AWS services and there are costs associated with these services after the Free Tier usage - please see the [AWS Pricing page](https://aws.amazon.com/pricing/) for details. You are responsible for any AWS costs incurred. No warranty is implied in this example.

## Requirements

* [Create an AWS account](https://portal.aws.amazon.com/gp/aws/developer/registration/index.html) if you do not already have one and log in. The IAM user that you use must have sufficient permissions to make necessary AWS service calls and manage AWS resources.
* [AWS CLI](https://docs.aws.amazon.com/cli/latest/userguide/install-cliv2.html) installed and configured
* [Git Installed](https://git-scm.com/book/en/v2/Getting-Started-Installing-Git)
* [AWS Serverless Application Model](https://docs.aws.amazon.com/serverless-application-model/latest/developerguide/serverless-sam-cli-install.html) (AWS SAM) installed
* [Rust](https://www.rust-lang.org/) 1.56.0 or higher
* [CargoLambda](https://www.cargo-lambda.info/guide/installation.html)
* Make sure to run "rustup target add aarch64-unknown-linux-gnu;"

## Deployment Instructions

1. Create a new directory, navigate to that directory in a terminal and clone the GitHub repository:
    ``` 
    git clone https://github.com/aws-samples/serverless-patterns
    ```
2. Change directory to the pattern directory:
    ```
    cd cloudfront-lambda-url-rust
    ```
3. Install dependencies and build:
    ```
    sam build
    ```
4. From the command line, use AWS SAM to deploy the AWS resources for the pattern as specified in the template.yml file:
    ```
    sam deploy
    ```
5. During the prompts:
    * Enter a stack name
    * Enter the desired AWS Region
    * Allow SAM CLI to create IAM roles with the required permissions.

    Once you have run `sam deploy -guided` mode once and saved arguments to a configuration file (samconfig.toml), you can use `sam deploy` in future to use these defaults.

6. Note the outputs from the SAM deployment process. These contain the resource names and/or ARNs which are used for testing.
   

## How it works

This pattern deploys an Amazon Cloudfront distribution, an AWS Lambda function URL. The function logs the incoming event and context object to an Amazon CloudWatch Logs log group and returns basic information about the event to the caller.

## Testing

Once the application is deployed, retrieve the CloudFront value from CloudFormation Outputs. Either browse to the endpoint in a web browser or call the endpoint from Postman.

Example GET Request: curl --location 'https://{DistributionDomainName}t?allowed_query_string_param=Daniele'

Response:
```
hello {IP}
```

If you check the headers back from the call you will see:
X-Cache: Miss from cloudfront

If you try again:
X-Cache: Hit from cloudfront

Example GET Request: curl --location 'https://{DistributionDomainName}t?allowed_query_string_param=Daniele'

Response:
```
hello Daniele ip: {IP}
```

If you check the headers back from the call you will see:
X-Cache: Miss from cloudfront

If you try again:
X-Cache: Hit from cloudfront


## Documentation
- [Announcing AWS Lambda Function URLs](https://aws.amazon.com/blogs/aws/announcing-aws-lambda-function-urls-built-in-https-endpoints-for-single-function-microservices/)http-api-develop-integrations-lambda.html)
- [AWS Lambda - the Basics](https://docs.aws.amazon.com/whitepapers/latest/serverless-architectures-lambda/aws-lambdathe-basics.html)
- [Lambda Function Handler](https://docs.aws.amazon.com/whitepapers/latest/serverless-architectures-lambda/the-handler.html)
- [Function Event Object - Overview](https://docs.aws.amazon.com/whitepapers/latest/serverless-architectures-lambda/the-event-object.html)
- [Function Context Object - Overview](https://docs.aws.amazon.com/whitepapers/latest/serverless-architectures-lambda/the-context-object.html)
- [Function Environment Variables](https://docs.aws.amazon.com/lambda/latest/dg/configuration-envvars.html)
- [Amazon CloudFront](https://docs.aws.amazon.com/AmazonCloudFront/latest/DeveloperGuide/Introduction.html)

## Cleanup
 
1. Delete the stack
    ```bash
    sam delete
    ```
2. Confirm the stack has been deleted
    ```bash
    aws cloudformation list-stacks --query "StackSummaries[?contains(StackName,'STACK_NAME')].StackStatus"
    ```

----
Copyright 2021 Amazon.com, Inc. or its affiliates. All Rights Reserved.

SPDX-License-Identifier: MIT-0
