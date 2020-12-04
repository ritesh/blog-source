+++
categories = ["til", " aws", " lambda"]
date = "2019-02-12T14:50:01+00:00"
summary = "tl;dr Testing a SAM lambda locally"
title = "Testing an AWS Lambda function locally"
url = "testing-aws-lambda-locally"

+++

Never had to test AWS Lambda functions locally (test in production YOLO).  I've always ended up writing toy functions that I test using the console. For functions that are non-trivial and have to process different kinds of input events, it's probably best to test them locally to discover any regressions early on.

While walking through the docs with a colleague, we found that the nifty way to test a lambda function created [using SAM](https://docs.aws.amazon.com/lambda/latest/dg/serverless_app.html) is to run:

`$ sam local start-lambda`

In the directory where you have your SAM `template.yml`. This is `LambdaInvoking` the function, as opposed to passing it an event via API Gateway. From the docs, write a quick script like this to then invoke the Lambda.

    import boto3
    import botocore
    
    # Set "running_locally" flag if you are running the integration test locally
    running_locally = True
    
    if running_locally:
    
        # Create Lambda SDK client to connect to appropriate Lambda endpoint
        lambda_client = boto3.client('lambda',
            region_name="eu-west-1",
            endpoint_url="http://127.0.0.1:3001",
            use_ssl=False,
            verify=False,
            config=botocore.client.Config(
                signature_version=botocore.UNSIGNED,
                read_timeout=30,
                retries={'max_attempts': 0},
            )
        )
    else:
        lambda_client = boto3.client('lambda')
    
    
    # Invoke your Lambda function as you normally usually do. The function will run
    # locally if it is configured to do so
    response = lambda_client.invoke(FunctionName="HelloFunction")
    
    # Verify the response
    assert response == "Hello World"

This is from the docs, but it took me a while to figure out that `FunctionName` above needs to refer to the _logical_ name of your Lambda function in the CloudFormation template.

The first run will be slow as SAM will fetch the container for your function runtime, subsequent runs should be quicker (adjust `read_timeout`accordingly).

If you're a security nerd, the bits above that say `use_ssl=False` and `verify=False` should give you pause. In this instance, you're connecting to a service locally and not reaching out to the internet, so you're OK.
